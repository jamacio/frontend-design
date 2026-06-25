# Agent Adapters

**Parent:** [`spec.md`](spec.md) · **Siblings:** [`architecture.md`](architecture.md) · [`skills-protocol.md`](skills-protocol.md) · [`new-agent-runtime-acp.md`](new-agent-runtime-acp.md) · [`modes.md`](modes.md)

The adapter layer is OD's most load-bearing design decision. We delegate the **entire agent loop** — model calls, tool use, context management, permission handling, resume, cancel — to the user's existing code agent CLI. OD's job is to detect it, feed it a skill + prompt + working directory, and stream its output back to the web UI.

If you're adding a new ACP-backed runtime, start with [`new-agent-runtime-acp.md`](new-agent-runtime-acp.md) for the expected stdio transport, JSON-RPC message flow, and process lifecycle contract.

> **Thesis:** The code agent space has already converged on a few strong implementations (code agent, code agent, Devin for Terminal, code editor Agent, AI assistant CLI, OpenCode, OpenClaw, Qoder CLI). Reimplementing another one is worse than just talking to all of them.
>
> **Inspiration:** [multica] (PATH-scan detection + daemon architecture) and [cc-switch] (per-agent config format knowledge + symlink-based skill distribution).

---

## 1. Adapter interface (TypeScript)

Every adapter implements this interface. The current adapter implementation lives in [`apps/daemon/src/agents.ts`](../apps/daemon/src/agents.ts).

```ts
interface AgentAdapter {
  readonly id: string;                      // "AI assistant-code" | "code agent" | …
  readonly displayName: string;

  // -- discovery --------------------------------------------------
  detect(): Promise<AgentDetection | null>; // null if not installed

  // -- capability negotiation ------------------------------------
  capabilities(): AgentCapabilities;

  // -- execution -------------------------------------------------
  run(params: AgentRunParams): AsyncIterable<AgentEvent>;
  cancel(runId: string): Promise<void>;
  resume?(runId: string, message: string): AsyncIterable<AgentEvent>;
}

interface AgentDetection {
  executablePath: string;                   // absolute path to CLI
  version: string;
  configDir?: string;                       // e.g. ~/.AI assistant
  skillsDir?: string;                       // e.g. ~/.AI assistant/skills
  authState: "ok" | "missing" | "expired";
}

interface AgentCapabilities {
  surgicalEdit: boolean;                    // can edit a targeted region without rewriting file
  nativeSkillLoading: boolean;              // picks up ~/.<agent>/skills/ automatically
  streaming: boolean;                       // emits tool calls in real time
  resume: boolean;                          // can continue an interrupted run
  permissionMode: "strict" | "permissive" | "none";
  contextWindowHint?: number;               // in tokens
}

interface AgentRunParams {
  runId: string;
  cwd: string;                              // absolute path — artifact dir
  systemPrompt: string;                     // skill's SKILL.md body + DESIGN.md excerpt
  userPrompt: string;
  skillDir?: string;                        // if set, adapter should make skill files available
  allowedTools?: string[];                  // for agents that support it
  timeoutMs?: number;
}

type AgentEvent =
  | { type: "thinking"; text: string }
  | { type: "tool_call"; name: string; input: unknown; id: string }
  | { type: "tool_result"; id: string; output: unknown }
  | { type: "text_delta"; text: string }
  | { type: "file_write"; path: string }   // synthesized by adapter if agent doesn't emit natively
  | { type: "error"; error: string }
  | { type: "done"; reason: "completed" | "cancelled" | "error" };
```

## 2. Detection strategy

Run all adapters' `detect()` in parallel on daemon start, then cache results in `~/.frontend-design/agents.json` with a 24h TTL. Re-detect on daemon `SIGHUP`.

Each adapter uses **two signals**:

1. **PATH scan.** `which <binary>` for each known executable name. Fast (<10ms).
2. **Config-dir probe.** Check for `~/.AI assistant/`, `~/.code agent/`, `~/.code editor/`, etc. This catches cases where the CLI was installed via npm global into a shell-specific PATH.

If both signals agree, detection is confident. If only one signal fires, we mark `authState: "missing"` and prompt the user to run the CLI's auth flow.

## 3. Adapter catalog (v1 target)

| Adapter | CLI command | Config dir | Skills dir | Native skill loading | Surgical edit | Streaming | Priority |
|---|---|---|---|---|---|---|---|
| **AI assistant-code** | `AI assistant` | `~/.AI assistant/` | `~/.AI assistant/skills/` | ✅ | ✅ | ✅ | P0 (MVP) |
| **amp** | `amp` | `~/.config/amp/` | n/a (via `amp skill add`) | ❌ (prompt-injected) | ✅ | ✅ (`-x --stream-json`, AI assistant-compatible) | P2 |
| **api-fallback** | *(direct AI API)* | — | — | ❌ (prompt-injected) | 〜 | ✅ | P0 (MVP) |
| **code agent** | `code agent` | `~/.code agent/` | `~/.code agent/skills/` | 〜 (varies by version) | 〜 (regenerate w/ scoping) | ✅ | P1 |
| **devin** | `devin` | `~/.config/devin/` | `~/.config/devin/skills/` | ✅ | ✅ | ✅ (`acp-json-rpc`) | P1 |
| **code editor-agent** | `code editor-agent` | `~/.code editor/` | n/a (via project `.code editorrules`) | ❌ (prompt-injected) | ✅ | ✅ | P1 |
| **AI assistant-cli** | `AI assistant` | `~/.config/AI assistant/` | ❌ | ❌ (prompt-injected) | ❌ (regenerate) | ✅ | P2 |
| **opencode** | `opencode` | `~/.opencode/` | 〜 | 〜 | ✅ | P2 |
| **openclaw** | `openclaw` | `~/.openclaw/` | 〜 | 〜 | 〜 | P2 |
| **AI assistant** | `AI assistant` | `~/.AI assistant/` | ❌ | ✅ (`edit` tool) | ✅ (`--output-format json` JSONL) | P2 |
| **kiro** | `kiro-cli` | `~/.kiro/` | ❌ | ✅ | ✅ (`acp-json-rpc`) | P2 |
| **kilo** | `kilo` | — | ❌ | ✅ | ✅ (`acp-json-rpc`) | P2 |
| **vibe** | `vibe-acp` | `~/.vibe/` | ❌ | ✅ | ✅ (`acp-json-rpc`) | P2 |
| **trae-cli** | `traecli` | Trae CLI config | Trae CLI managed | ❌ (prompt-injected) | ✅ | ✅ (`acp-json-rpc`) | P2 |
| **deepseek** | `deepseek` | `~/.deepseek/` | `~/.deepseek/skills/` | ❌ (prompt-injected) | ✅ | ✅ (plain text) | P2 |
| **qoder** | `qodercli` | Qoder CLI config | Qoder CLI managed | ❌ (prompt-injected) | ✅ | ✅ (`stream-json`) | P2 |
| **pi** | `pi` | `~/.pi/agent/` | `~/.pi/agent/skills/` | ❌ (prompt-injected) | ✅ | ✅ (`pi-rpc` JSON-RPC) | P2 |

"P0/P1/P2" correspond to the roadmap phases in [`roadmap.md`](roadmap.md).

## 4. Skill injection per adapter

Skills travel into each agent via one of three strategies, in order of preference:

### 4.1 Native skill loading (preferred)
Agent scans its own `~/.<agent>/skills/` on launch. We symlink OD's skill into that dir (see [`skills-protocol.md`](skills-protocol.md) §3) and let the agent pick it up natively. Zero prompt overhead.

- **Works for:** code agent. code agent (version-dependent). OpenCode.

### 4.2 Prompt injection (fallback)
We read the skill's `SKILL.md` body + any `references/*.md` files it has, concatenate them into the system prompt, and copy `assets/` files into the cwd. The agent has no concept of "skills" but has the instructions.

- **Works for:** everyone. Default for API fallback, code editor Agent, AI assistant CLI.
- **Cost:** more tokens per run. Mitigation: prune `references/` to the files the skill body actually mentions.

### 4.3 File-placed workflow (hybrid)
For agents that support `AGENTS.md` / `.code editorrules` / similar project-level instruction files (code editor Agent, OpenCode), we write a project-scoped instruction file in the artifact cwd before running the agent. The agent picks it up automatically.

- **Works for:** code editor Agent (`.code editorrules`), some OpenCode configurations.

The adapter declares which strategy to use via `capabilities().nativeSkillLoading` and a private `skillInjectionStrategy` field.

## 5. Per-adapter notes

### 5.1 code agent (reference implementation)

- Invocation: `AI assistant --print --output-format stream-json --cwd <artifact-dir> "<prompt>"`.
- Streaming format: JSON Lines over stdout; each line is an event we can map to `AgentEvent` directly.
- Skill loading: native. Just ensure the skill is symlinked in `~/.AI assistant/skills/`.
- Surgical edits: use the `Edit` tool; code agent's own loop handles this.
- Permission: set `--allowed-tools "Read,Edit,Write"` to restrict blast radius.
- Cancel: send `SIGTERM`; code agent flushes and exits.
- **Gotchas:** code agent's JSON stream schema is versioned — pin to a known version, warn on mismatch.

### 5.2 API fallback (no CLI)

- Invocation: direct AI Messages API with `stream: true`.
- Skill loading: prompt injection only — read the skill dir, inline everything.
- Tool use: we register `Read/Write/Edit` as tools, implement them in the daemon against the artifact cwd, and run the loop ourselves. This is the one place OD does own the loop — because the user has no agent at all. Keep it as dumb as possible.
- Surgical edits: approximated by regenerating the whole target file with "only change X" in the prompt.
- Model: AI assistant deepseek 4.6 default; deepseek 4.7 behind a flag.
- **Why ship this at all?** Topology C requires it (no daemon available in a pure-Vercel deploy). Also, users trying OD for the first time without a CLI installed still get a working experience.

### 5.3 code agent

- Invocation: `code agent exec --cwd <dir> "<prompt>"`.
- Streaming: line-based; parse with a regex-based state machine. Less rich than code agent's JSON stream.
- Skill loading: varies. Newer code agent versions read `~/.code agent/skills/`; older versions don't. Detect by version string; fall back to prompt injection.
- Surgical edits: code agent's edit tool exists but the tool-call schema is different enough that we regenerate files instead in v1. Revisit in v2.
- **Gotcha:** code agent's CLI auth state can be "authenticated to wrong org." Detect by running `code agent whoami` at detect time.

### 5.4 Devin for Terminal

- Invocation: `devin --permission-mode dangerous --respect-workspace-trust false acp`.
- Install/update: macOS/Linux/WSL users can install with `curl -fsSL # | bash`; run `devin update` for existing installs.
- Version requirement: requires a Devin CLI build with the `devin acp` subcommand (verified with `devin 2026.5.1-1`). Check with `devin acp --help`; if the subcommand is missing, update or reinstall Devin for Terminal.
- Streaming: Agent Client Protocol JSON-RPC over stdio, handled by the daemon's shared `acp-json-rpc` transport.
- Skill loading: Devin supports `.devin/skills/` and `~/.config/devin/skills/`; OD's current daemon also prompt-injects the selected skill body into the composed prompt, so no per-project skill install is required for generation.
- Surgical edits: Devin's own edit/write tools handle targeted changes.
- Permission: `--permission-mode dangerous` avoids headless approval prompts in the web UI; `--respect-workspace-trust false` ensures Devin doesn't block on trust prompts for newly created project dirs. Org/team-level policies still apply inside Devin.

### 5.5 code editor Agent

- Invocation: `code editor-agent --workspace <dir> "<prompt>"` (rough; verify with CLI docs at implementation time).
- Streaming: yes, JSON lines.
- Skill loading: no native skill concept. We write a `.code editorrules` file into the artifact dir before running. The rules file contains the skill's SKILL.md body (minus front-matter).
- Surgical edits: code editor's inline edit tool is strong; map our `refine` call to its edit protocol.
- **Gotcha:** code editor Agent operates on workspaces, not single files. Constrain the workspace to the artifact dir to prevent over-broad changes.

### 5.6 AI assistant CLI

- Invocation: `AI assistant` with the composed prompt delivered via **stdin** (no `-p` flag).
  AI assistant CLI enters headless mode automatically when stdin is a pipe and no `-p` flag is
  supplied — verified with `AI assistant@0.1.x`.
- Trust: `AI assistant_CLI_TRUST_WORKSPACE=true` is set in the spawned process instead of
  passing `--skip-trust`, which is version-fragile across AI assistant CLI releases.
- Streaming: yes, `--output-format stream-json` to stdout.
- Skill loading: prompt injection only.
- Surgical edits: regenerate whole file.
- **Gotcha — `spawn ENAMETOOLONG` on Windows:** Passing the full composed prompt as a
  `-p <string>` CLI argument hits Windows' `CreateProcess` hard limit of ~32 KB for the
  entire command line. The fix is to set `promptViaStdin: true` in the agent definition
  and write the prompt to `child.stdin` after spawning. The daemon's `/api/chat` handler
  checks this flag and opens stdin as a pipe instead of `'ignore'`.
- **Gotcha:** AI assistant's tool-use format is distinct; we translate our file-write tool to its
  `file_tool` equivalent when that feature is implemented.

### 5.7 OpenCode / OpenClaw

- Less-matured CLIs. Targeting P2. Expect bumps; adapter implementations will likely be the thinnest possible "shell out, parse output, synthesize events" approach.

### 5.8 GitHub AI assistant CLI

- Invocation: `AI assistant -p "<prompt>" --allow-all-tools --output-format json --add-dir <skills> --add-dir <design-systems>`. `--allow-all-tools` is mandatory in non-interactive mode — without it the CLI blocks waiting for human approval on every tool call. Unlike code agent (where `exec` is a dedicated headless subcommand with auto-approve baked in) or code agent (which inherits its permission policy from `~/.AI assistant/settings.json`), AI assistant's `-p` mode always prompts unless this flag is passed explicitly. `--add-dir` (repeatable) widens the path-level sandbox so AI assistant can read skill seeds and design-system specs that live outside the project cwd.
- Streaming: `--output-format json` emits JSONL with the same expressive shape as code agent's stream-json (`assistant.reasoning_delta`, `assistant.message_delta`, `tool.execution_start/complete`, `result`). `apps/daemon/src/AI assistant-stream.ts` maps these onto the same UI events as `AI assistant-stream.ts`.
- Skill loading: prompt injection only. Github AI assistant's tool catalog includes a `skill` tool — native format worth reverse-engineering later.
- Surgical edits: dedicated `edit` tool.
- Detection assumes AI assistant is already authenticated, via one of: `AI assistant login` (subcommand, OAuth device flow), the interactive `/login` slash command inside `AI assistant` with no args.

### 5.9 Qoder CLI

- Invocation: `qodercli -p --output-format stream-json --permission-mode bypass_permissions --cwd <dir> [--model <id>] --add-dir <absolute-skills-dir> --add-dir <absolute-design-systems-dir>`, with the composed prompt delivered over stdin. Print mode exits after the turn, which fits the daemon's one-request chat lifecycle. Qoder is currently text-only in OD; `_imagePaths` are intentionally ignored because Qoder CLI does not expose a supported multimodal flag for this adapter path yet.
- Streaming: `--output-format stream-json` emits JSONL records such as `system/init`, `assistant`, and `result`. `apps/daemon/src/qoder-stream.ts` maps assistant content blocks to text deltas, maps assistant errors without text to typed error events, and preserves result usage, model usage, cost, duration, stop reason, and unknown records as raw events.
- Models: ships fallback hints for `default`, `lite`, `efficient`, `auto`, `performance`, and `ultimate`. Selecting `default` omits `--model` so Qoder's own CLI configuration remains authoritative.
- Skills: prompt injection only in v1. `--add-dir` is repeatable so Qoder can read absolute skill and design-system roots that live outside the active project cwd; the daemon does not forward relative extra directory entries.
- Permission: `--permission-mode bypass_permissions` avoids headless approval prompts in the web UI. Users should treat this as the same trust posture as running Qoder directly with that flag in the selected project directory.
- **Gotcha:** Detection only proves `qodercli --version` can run. Qoder authentication and account scope remain owned by Qoder CLI, with credentials read from Qoder's `~/.qoder/config.json`; the daemon surfaces stderr/stdout failures from the spawned run instead of running login or editing Qoder config.

### 5.10 Trae CLI

- Invocation: `traecli acp serve --yolo`, using the daemon's shared ACP JSON-RPC transport. The adapter follows Trae CLI's public ACP entrypoint documented at #
- Streaming: `acp-json-rpc`; the daemon uses the same ACP event path as the other ACP-backed adapters.
- Models: dynamic via the ACP handshake. If model discovery fails, the picker falls back to the CLI's default configuration rather than requiring CI or startup detection to log in to Trae CLI.
- Skills: prompt injection only in v1. External tool servers can be forwarded through the ACP launch descriptor with the existing `acp-merge` path.
- Permission: `--yolo` avoids headless approval prompts in the web UI. This follows the adapter catalog's existing non-interactive permission posture for CLIs such as Devin, AI assistant, Qoder, and deepseek: the daemon runs agent CLIs without a TTY, so it must not rely on an interactive tool-approval prompt to make progress.
- **Gotcha:** Detection only proves `traecli --version` and model discovery can run in the current environment. Trae CLI owns login, account scope, and model entitlement; the daemon does not run login flows or edit Trae CLI configuration.

### 5.11 Pi

- Invocation: `pi --mode rpc [--model <id>] [--thinking <level>] [--append-system-prompt <dir> …]`, with the composed prompt delivered over stdin via JSON-RPC. The daemon sends a `prompt` command (optionally with `images` for multimodal input) and pi streams back typed events until `agent_end`. Pi's RPC process stays alive after `agent_end` (designed for multi-prompt sessions); the daemon closes stdin and SIGTERMs after a grace period since `/api/chat` is single-shot.
- Streaming: `pi-rpc` JSON-RPC over stdio. Events include `agent_start`, `turn_start/end`, `message_update` (text deltas, thinking deltas, tool calls), `tool_execution_start/end`, `compaction_start`, `auto_retry_start/end`, `extension_error`. `apps/daemon/src/pi-rpc.ts` maps these onto the same UI event set as `AI assistant-stream.js` / `AI assistant-stream.js` / `acp.js`. Error events from `extension_error` and exhausted `auto_retry_end` are routed through `sendAgentEvent` so the daemon's empty-output guard and `agentStreamError` flag apply (same path as qoder-stream-json and json-event-stream after issue #691).
- Models: dynamic — `pi --list-models` prints a TSV table to stderr that the daemon parses into provider/model picker entries. Fallback hints for the most common providers/models are shipped for when the list command times out.
- Images: pi's RPC `prompt` command supports an `images` field (base64-encoded `ImageContent` objects). The daemon reads validated `imagePaths` at session attach time and includes them in the prompt command. Unreadable images are skipped rather than failing the run.
- Skills: prompt injection in v1. `extraAllowedDirs` (skill seed and design-system directories) are forwarded as `--append-system-prompt` repeatable flags so the agent knows these directories exist and can Read files inside them. pi doesn't have an `--add-dir` sandbox flag — it uses OS cwd — so system-prompt hints are the only available mechanism. **Important:** `--append-system-prompt` only hints paths in the system prompt; it does not grant sandbox or filesystem access. pi's Read tool can normally open absolute paths outside cwd, but when absolute reads fail (sandboxed environments, restricted permissions), the reliable fallback is to stage copies of the needed files into the project cwd before the run. No stronger pi flag exists for this purpose today.
- Thinking: the daemon exposes pi's `--thinking` levels (`off`, `minimal`, `low`, `medium`, `high`, `xhigh`) in the Settings model picker.
- Extension UI: auto-resolved. pi's RPC protocol can request user dialogs (`select`, `confirm`, `input`, `editor`) and fire-and-forget notifications (`setStatus`, `setWidget`, `notify`, `setTitle`, `set_editor_text`). Dialog methods are auto-approved (confirm → true, select → first option) and fire-and-forget methods are silently consumed because the web UI has no surface for them.
- **Gotcha:** pi's RPC `prompt` response is asynchronous — `success: true` only means the prompt was accepted, not that the agent finished. Agent failures after acceptance surface through the normal event stream (`extension_error`, `auto_retry_end` with `success: false`) and the empty-output guard.

### 5.12 DeepSeek TUI

- Invocation: `deepseek exec --auto [--model <id>] "<prompt>"`. The `deepseek` dispatcher owns the `exec` / `--auto` subcommands and delegates to a sibling `deepseek-tui` runtime binary at exec time; upstream documents both binaries as required (the npm and cargo paths install them together). We only probe the dispatcher — `deepseek-tui` on its own doesn't accept this argv shape, so advertising it as a fallback would surface the agent as available but fail on the first chat run. A future revision could teach resolution + buildArgs which binary was selected and emit a verified `deepseek-tui` invocation, with a regression test exercising that path.
- Streaming: plain text deltas to stdout in non-`--json` mode (tool-call notifications go to stderr). Skipping `--json` is intentional — `deepseek exec --json` batches the entire run into one trailing summary object instead of streaming, which would freeze the chat UI until end-of-turn.
- Auto-approval: `--auto` enables agentic mode with the YOLO permission posture. The daemon runs every CLI without a TTY, so the interactive approval prompt would otherwise hang the run.
- Skills: prompt injection only in v1. DeepSeek TUI does walk `.agents/skills`, `skills`, `.opencode/skills`, `.AI assistant/skills`, and `~/.deepseek/skills` first-wins, so a future revision can switch to file-placed skill loading the same way code agent does.
- Models: ships `deepseek-v4-pro` and `deepseek-v4-flash` as fallback hints (1M-token context windows, native thinking-mode streaming). Users can paste any other id (e.g. `nvidia-nim/deepseek-v4-pro`, `fireworks/deepseek-v4-flash`) via the Settings dialog's custom-model input.
- **Gotcha — auth state is not auto-detected.** DeepSeek TUI reads its API key from `~/.deepseek/config.toml` or `DEEPSEEK_API_KEY`. If the user hasn't run `deepseek auth set --provider deepseek` (or set the env var), the first run errors out with a non-actionable message. Detection currently only reports `available: true` based on the binary being on PATH; surface auth state via `deepseek doctor --json` in a follow-up.

## 6. Capability-driven UI

The web UI reads `agents.capabilities()` and disables features that the active adapter can't support:

| UI feature | Requires | If missing |
|---|---|---|
| Comment mode (click to refine) | `surgicalEdit: true` | Hidden; show tooltip explaining why |
| Streaming tool-call feed | `streaming: true` | Show a spinner only |
| Resume interrupted run | `resume: true` | "Cancel + restart" only |
| Skill picker shows skill with `od.capabilities_required` | all listed caps | Skill greyed out with reason |

This is how we avoid "works on my code agent, breaks on your AI assistant" — we detect, degrade, and document.

## 7. Agent switching

The user can switch active agent per session:

```
POST agents.setActive { agentId: "code agent" }
→ capabilities() reported
→ web UI refreshes feature gates
→ next generation runs on code agent
```

Switching mid-run is not allowed (cancel first). The artifact is agent-agnostic; only the generation process differs.

## 8. Fallback chain

If the user's preferred agent fails (crash, auth, timeout), OD offers a one-click fallback in this order:

1. User's preferred agent (e.g. code editor Agent)
2. Any other detected agent (code agent, if installed)
3. API fallback (direct AI, requires key)

The user explicitly opts in to fallback — we don't silently switch, because a skill may have been authored for a specific agent's capabilities.

## 9. Detection UX

First run:

```
$ pnpm tools-dev run web
[od] daemon starting on :7456
[od] detecting agents…
[od]   ✓ AI assistant-code v0.6.3 (auth: ok, skills dir linked)
[od]   ✓ code agent v0.8.1 (auth: ok)
[od]   ✗ code editor-agent (not installed)
[od]   ✗ AI assistant-cli (installed but not authenticated; run `AI assistant auth login`)
[od]   ✓ api-fallback (AI_API_KEY found)
[od] daemon ready; 3 agents available
```

Web UI mirrors this in an agent-selector dropdown, with unauthenticated agents shown greyed out with a fix-it tooltip.

## 10. Authorization boundaries

We inherit the underlying agent's permission model rather than building our own. This means:

- **code agent** respects its own `--allowed-tools` and `--permission-mode` flags. OD passes through user preferences.
- **code agent / code editor** sandbox by workspace; OD always sets cwd to the artifact dir so nothing outside is visible by default.
- **Qoder CLI** runs with `--permission-mode bypass_permissions` for non-interactive web execution and is scoped by the daemon's cwd plus explicit absolute `--add-dir` entries.
- **API fallback** is the one case we own. We implement a whitelist: only `Read`, `Write`, `Edit` tools, all rooted at the artifact cwd. Network access is off.

The daemon never grants more authority to an agent than it had on its own. We don't run the agent in a privileged mode "for convenience."

## 11. Adapter source layout

```
apps/daemon/
├── base.ts                 # shared interface + utility helpers
├── AI assistant-code/
│   ├── adapter.ts
│   ├── stream-parser.ts    # JSON-lines → AgentEvent
│   └── detect.ts
├── api-fallback/
│   ├── adapter.ts
│   ├── tool-loop.ts        # the minimal tool-use loop
│   └── tools.ts            # Read/Write/Edit implementations
├── code agent/                  # Phase 1
├── code editor-agent/           # Phase 1
├── AI assistant-cli/             # Phase 2
├── opencode/               # Phase 2
└── openclaw/               # Phase 2
```

Each adapter is a separate module so community contributions can add new ones without touching core daemon code.

## 12. Open questions

- **Nested agents.** What if code agent's agent itself spawns a subagent? We receive events from the outer process only. v1 policy: surface only top-level events; summarize subagent activity as "sub-task" placeholder.
- **Billing awareness.** Some agents bill per message, some per token. OD doesn't track cost in MVP; v1 adds an optional "usage" event from adapters that expose it.
- **Windows support.** PATH scanning and `spawn` semantics differ on Windows. v1 targets
  macOS and Linux; Windows is best-effort. Known issue fixed: `spawn ENAMETOOLONG` when
  running AI assistant CLI (and other plain-text agents) on Windows — resolved by routing the
  composed prompt through stdin instead of as a CLI argument (see §5.5).
- **Docker-contained agents.** Some users run code agent in a container. Adapter needs a "remote" mode — probably same interface but talks over SSH. Phase 2+.
