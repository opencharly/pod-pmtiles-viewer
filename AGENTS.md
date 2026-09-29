# AGENTS.md — pod-pmtiles-viewer

Standalone candy repo for the `pmtiles-viewer` candy — the `protomaps/PMTiles`
visual archive inspector, built from upstream source via npm and served as a
static SPA on port `8001` (host `28001`). The candy lives in `charly.yml` at the
repo root. There is no source tree — the app is fetched and built at image build
time.

Canonical files:

- `charly.yml` — the `pmtiles-viewer:` candy entity (description, `require`,
  `distro`, `port`, `env_provide`, `service`, `plan`) and its `skill:` entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `CHANGELOG/` — per-CalVer history.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-versa:pmtiles-viewer` — the owning skill: layer properties, the Vite
  `--base=/` build override and the asset-base check, and the four sibling PMTiles
  archives this viewer inspects. Load before editing, building, deploying, or
  troubleshooting this candy.
- `/charly-versa:osm-tools-layer` — the companion martin tile server the viewer
  points at.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, service declarations).
- `/charly-check:check` — the check/R10 framework (`charly check box`,
  `charly check run <bed>`).
- `/charly-internals:git-workflow` — before any git/PR action.

## Build / validate / test

- `charly box validate` at the repo root — the structural check: the manifest
  must parse and validate at the installed charly.
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate. Its only workflow file is
  `.github/workflows/tag-on-merge.yml`.
- The candy's own `check:` steps assert the Vite build artifact and its
  `index.html`, the node toolchain, the running `pmtiles-viewer` service, the
  reachable port, `GET /` returning `200`, and root-relative asset URLs.

## Modify this repo

- Edit the `pmtiles-viewer:` candy entity in `charly.yml`; the `skill:` entity in
  the same file is the owning skill's source — a candy change and its skill change
  land together.
- The build pins the served asset base with `npm run build -- --base=/`; the
  `pmtiles-viewer-asset-base-not-prefixed` check locks it. Keep them in step.
- Keep the served directory (`/opt/pmtiles-viewer/build`) in step across the build
  step, the service `exec`, and the checks.
- The `skill:` entity is the source for `/charly-versa:pmtiles-viewer`; never edit
  the generated `SKILL.md` — regenerate it.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
