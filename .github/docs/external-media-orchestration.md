# External Media Orchestration

This note describes how an external service can use Frontend Design as a creative
runtime while keeping media provider governance outside Frontend Design.

Frontend Design contributes project context, skills, design systems, previews,
artifact structure, and design-aware prompt composition. The external service
owns caller auth, admission, templates, fanout, retries, accounting, webhooks,
provider credentials, budgets, model routing, and provider rate limits.

## Boundary

Run-scoped `mediaExecution` controls only Frontend Design-owned media generation.
It applies to:

- token-gated `/api/tools/media/generate`
- in-run `od media generate` when `OD_TOOL_TOKEN` is present
- Frontend Design's code agent image generation prompt override
- Frontend Design's media generation prompt contract

It intentionally does not apply to external MCP media tools. If a run receives
tool integrations from an external service, that service owns the provider policy for
those tools.

Frontend Design should not grow a generic provider router, provider account pool,
global media budget system, or external executor API unless there is a separate
owner decision for that product surface.

## Recommended Composition

Use HTTP/SSE between the external service and Frontend Design. Avoid shelling out to
`od` from the external service unless the integration specifically needs the CLI
contract.

1. The external service authenticates the caller and decides provider policy.
2. The external service creates or selects an Frontend Design project.
3. The external service starts a run with `mediaExecution` set to the desired
   Frontend Design-owned media policy.
4. The run includes skills and tool integrations that describe the external media
   workflow.
5. Frontend Design handles design/runtime work. External MCP media tools handle
   provider execution when the external service permits it.
6. The external service stores final provider outputs in the project through
   normal artifact or file APIs, or asks the agent to place returned assets in
   the project workspace.

For runs where the external service owns all provider execution, start with:

```json
{
  "agentId": "code agent",
  "projectId": "project_123",
  "conversationId": "conversation_123",
  "message": "Create a campaign concept and produce the requested media through the configured media tools.",
  "mediaExecution": {
    "mode": "disabled"
  }
}
```

For runs where Frontend Design may use only a narrow part of its own media path,
use allowlists:

```json
{
  "mediaExecution": {
    "mode": "enabled",
    "allowedSurfaces": ["image"],
    "allowedModels": ["AI-image-2"]
  }
}
```

## Skill Pattern

A skill should describe creative intent and routing expectations. It should not
embed provider credentials or provider-account policy.

Example `SKILL.md` fragment:

```md
# Media Campaign Skill

Use Frontend Design project context, design-system guidance, and artifact previews
to plan the campaign. When media bytes are needed, use the configured external
media tool integrations. Do not call Frontend Design-owned media generation unless the run
policy explicitly permits it.

For every generated asset, write a short project note that records:

- creative brief
- requested surface and dimensions
- provider-facing prompt summary
- returned asset path or URL
- usage constraints supplied by the external media tool
```

This keeps the skill focused on design workflow. The external tool remains the
authority for provider execution and fulfillment rules.

## tool integration Pattern

MCP is the preferred way to expose external media execution to an agent run
without making Frontend Design own provider auth or budgets.

A media tool server can expose tools such as:

- `media.describe_policy`: return available surfaces, model families, budget
  hints, and safety constraints for the current caller
- `media.submit`: submit a provider request and return an external request id
- `media.status`: poll or fetch completion state
- `media.cancel`: cancel a pending request when supported
- `media.attach_result`: return a URL or project-file placement instruction for
  completed media

These names are illustrative. The stable contract belongs to the tool server and
the external service, not to Frontend Design core.

The tool server should receive its own credentials from the external service or
from its deployment environment. Frontend Design should only see the tool integration
surface made available to the run.

## Artifact Handoff

Prefer one of these handoff shapes:

- The tool integration returns a downloadable URL and the agent writes or imports the
  asset into the project.
- The external service uploads the fulfilled asset through an Frontend Design
  project file/artifact API after provider completion.
- The tool integration returns metadata that the agent records in a project manifest or
  handoff note, while the external service keeps the provider artifact as the
  source of truth.

Avoid storing provider credentials, account ids, budget ids, or retry policy in
Frontend Design project files. Project artifacts should describe creative output,
not provider-account governance.

## Legacy Media Endpoint Caveat

`POST /api/projects/:id/media/generate` predates run-scoped media policy. It is
still available for normal Frontend Design media generation outside the in-run tool
path. The accepted v1 policy closes the cooperative in-run CLI path by routing
`od media generate` through `/api/tools/media/generate` when `OD_TOOL_TOKEN` is
present.

A raw local HTTP call from inside a run can still reach the legacy project route
without presenting the run token. That hardening question is tracked separately
in issue #3199 because it has product decisions around UI compatibility,
outside-run CLI behavior, and whether the legacy route should become
policy-aware or be reserved for non-run callers.

## Non-goals

This composition pattern does not add:

- `request-only` run mode in Frontend Design core
- Frontend Design media request persistence for external provider work
- a generic HTTP executor provider inside Frontend Design
- provider credentials or account pools in Frontend Design
- Frontend Design-owned provider budgets, retries, or global media limits
- tool integration routing based on `mediaExecution.allowedSurfaces` or
  `mediaExecution.allowedModels`

If MCP orchestration needs its own constraints later, add a separate explicit
contract for that surface instead of reusing `mediaExecution`.
