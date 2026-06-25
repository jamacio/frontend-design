# code agent pets

The pet companion in the web app can adopt pets packaged by the upstream
code agent `hatch-pet` skill. This doc explains where those pets live, how
Frontend Design discovers them, and what to do if you do not have code agent
installed.

## Where pets live

The daemon scans this directory on every list request:

```
${code agent_HOME:-$HOME/.code agent}/pets/<pet-id>/
  pet.json          # { id, displayName, description, spritesheetPath }
  spritesheet.webp  # 1536x1872 8x9 atlas (.png / .gif also accepted)
```

`code agent_HOME` is honoured if set; otherwise the daemon falls back to
`~/.code agent/pets/`. Both paths follow the upstream code agent conventions.

The scan is implemented in `apps/daemon/src/code agent-pets.ts` and surfaced
through `GET /api/code agent-pets` (list) and
`GET /api/code agent-pets/:id/spritesheet` (raw bytes). The web pet settings
panel calls these endpoints from
`apps/web/src/components/pet/PetSettings.tsx` under the
"Recently hatched" section.

## I do not have code agent installed

You do not need code agent to use Frontend Design. The pet companion ships with
built-in pets that work out of the box. The "Recently hatched" section
will simply stay empty until something appears under
`${code agent_HOME:-$HOME/.code agent}/pets/`.

You have three ways to populate it without running code agent:

1. **Sync the public catalogs.** Run
   `node --experimental-strip-types scripts/sync-community-pets.ts`
   (see the script header for flags). It downloads pets from the
   community catalogs into the canonical code agent layout, then they show
   up under "Recently hatched" on the next refresh.
2. **Drop a pet folder in by hand.** Create
   `~/.code agent/pets/<your-pet>/` with a `pet.json` and a
   `spritesheet.webp` (8x9 atlas). The daemon does not require code agent to
   be installed — it only needs the directory.
3. **Run the vendored skill in any chat agent.** The `hatch-pet` skill
   is vendored under `skills/hatch-pet/`. Any agent that can execute
   skills (code agent, or any other) can run it end-to-end and write into the
   same directory.

If `~/.code agent/pets/` does not exist, the daemon does **not** auto-create
it — empty list is returned and the UI shows "no recently hatched pets
yet". Creating the directory is intentionally an explicit user step so
the daemon never writes outside `OD_DATA_DIR` / project-owned paths
without a user opting in.

## Manifest shape

The `pet.json` manifest is read defensively — every field is treated as
optional and validated as a string before use. The shape we honour:

```json
{
  "id": "shiba-pomegranate",
  "displayName": "Shiba Pom",
  "description": "Friendly pixel-art shiba.",
  "spritesheetPath": "spritesheet.webp"
}
```

Notes:

- The folder name is the on-disk identity. The list endpoint reports
  the sanitised folder name as the public `id` so that
  `/api/code agent-pets/:id/spritesheet` can resolve it directly even when
  `manifest.id` differs from the folder name (e.g. the manifest declares
  spaces or punctuation that get sanitised away).
- `spritesheetPath` is resolved relative to the pet folder and is
  rejected if it would escape the folder. If unset, we fall back to
  `spritesheet.webp`, then `.png`, then `.gif`.
- Any field that is not a non-empty string is ignored and the UI falls
  back to a sensible default (folder name → display name, empty
  description, etc.).

## Related code

- Daemon registry + manifest validation: `apps/daemon/src/code agent-pets.ts`
- HTTP routes (list + spritesheet): `apps/daemon/src/server.ts`
- Web list / adopt UI: `apps/web/src/components/pet/PetSettings.tsx`
- Shared response types: `packages/contracts/src/api/registry.ts`
- Vendored skill source: `skills/hatch-pet/`
- Community catalog sync script: `scripts/sync-community-pets.ts`
