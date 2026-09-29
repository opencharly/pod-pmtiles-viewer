# pod-pmtiles-viewer

The `pmtiles-viewer` candy of the OpenCharly candy library, as a standalone repo
(the candy de-submodule cutover, kind-prefixed naming). It serves the
[PMTiles](https://github.com/protomaps/PMTiles) visual archive inspector as a
static single-page app.

## What it provides

Builds the `protomaps/PMTiles` TypeScript SPA — the same code deployed at
[pmtiles.io](https://pmtiles.io) — from upstream source via npm at image build
time, and serves the Vite `dist/` as a static site with Python's stdlib
`http.server`. Operators point it at any `.pmtiles` archive (for example one
served by the `pod-osm-tools` martin tile server) to inspect its bbox, zoom
range, metadata, and tile contents in the browser.

| Property | Value |
|---|---|
| Port | `8001` (host-mapped to **28001**) |
| Service | `pmtiles-viewer` (`python3 -m http.server 8001 --directory /opt/pmtiles-viewer/build`, `restart: always`, priority 35) |
| Requires | `layer-supervisord` |
| env_provide | `PMTILES_VIEWER_PUBLIC_URL` (`http://127.0.0.1:{{.HostPort 8001}}`) |
| Static dist | `/opt/pmtiles-viewer/build/` |
| Distros | `arch`, `fedora` (node + npm + git for the build) |

## How to use it

```yaml
my-viewer:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/pod-pmtiles-viewer:<tag>'
```

```bash
charly box build my-viewer
charly start my-viewer
# open http://localhost:28001
```

Use the viewer's "Load remote archive" input to point it at a PMTiles URL, e.g.
`http://127.0.0.1:23000/monaco-gpqtiles`.

## Layout

- `charly.yml` — the `pmtiles-viewer:` candy entity (description, `require`,
  `distro`, `port`, `env_provide`, `service`, `plan`) plus its `skill:` entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `CHANGELOG/` — per-CalVer history.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-versa:pmtiles-viewer` — the layer properties, the Vite
  `--base=/` override, and the sibling PMTiles archives it inspects.
- `/charly-versa:osm-tools-layer` — the companion martin tile server.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
