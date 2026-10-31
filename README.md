<!-- victorious-fleet-readme-refresh:start -->
<p align="center">
  <img src="assets/logo.jpg" alt="fleet-manifest logo" width="200">
</p>

# fleet-manifest

🛍️ **[ShopVictorious](https://victorious93.github.io/fleet-manifest/shop/)** — Victorious Apparel merch (buy links coming soon)

Fleet APK update feed and status/planning docs for the Victorious fleet.

**Package id:** `n/a (manifest)`

**Status:** CI green (docs/feed) *(from [fleet-manifest ROADMAP](https://github.com/Victorious93/fleet-manifest/blob/main/ROADMAP.md))*

## Install / build

No app build. Authoritative feed: `fleet-manifest.json`; status: `ROADMAP.md`.

_Steps above are taken from this repo’s build files / existing docs; anything not exercised in this refresh is unlabeled as verified on a device._

## Victorious fleet

- Monorepo: [https://github.com/Victorious93/victorious-fleet](https://github.com/Victorious93/victorious-fleet)
- Fleet site: [https://victorious-framework.pages.dev](https://victorious-framework.pages.dev)

<!-- victorious-fleet-readme-refresh:end -->

Public index of the latest Android APKs across Don’s Victorious fleet apps: versions, download URLs, content SHA-256, signer cert, and debug/release flags.

**Live URL (GitHub Pages):** https://victorious93.github.io/fleet-manifest/fleet-manifest.json

**Raw fallback:** https://raw.githubusercontent.com/Victorious93/fleet-manifest/main/fleet-manifest.json

## Trust model (important)

There is **no** top-level shared signing cert in the manifest.

- Each published app entry has `certSha256` (actual signer of the APK bytes, or `null` if not yet computed).
- Each app in [`apps.yml`](apps.yml) has a `pinnedCertSha256` **allow-list** (may be empty = unknown).
- **Clients MUST trust an entry only when `certSha256` is non-null and matches that app’s pin** from the controller pairing table **or** the `pinnedCertSha256` list in `apps.yml`. Do not trust on `applicationId` alone.

Only **BOUND_COMPANION** apps share the fleet release cert  
`9E:A7:B3:14:94:5B:0A:62:97:15:6F:F7:06:E7:0C:0A:B1:B7:24:DE:06:D5:09:9C:A5:12:8A:B4:43:F7:2D:8D`:

- HackerAI-Commander
- Pentest-Swarm-AI
- EDL-Commander

All other apps have empty pins until Don provides them. Do not invent pins.

## Schema

See [`schema.json`](schema.json) (JSON Schema draft 2020-12).

Per app (required): `id`, `name`, `repo`, `applicationId`, `versionName`, `versionCode`, `debugBuild`, `releaseTag`, `releaseNotesUrl`, `publishedAt`, `certSha256` (string or null), `assets[]`.

Optional: `minSdk`, `buildType` (`debug` | `release`).

`debugBuild` is derived from `aapt2 dump badging` (`application-debuggable`) and/or tag/asset names containing `debug`.

## Updater

[`.github/workflows/update-manifest.yml`](.github/workflows/update-manifest.yml):

- Triggers: **daily cron** + **workflow_dispatch** (no PAT / no `repository_dispatch`)
- Uses `GITHUB_TOKEN` with `permissions: contents: write`
- For each public app in `apps.yml`: Releases API → download APKs → asset digest or `sha256sum` → `aapt2 dump badging` → `apksigner verify --print-certs` → validate → commit if changed

Private apps (VictoryPed, victormodem, etc.) are not covered ($0 public Actions only). victorsms / victorlaunch / victorspoof are now PUBLIC and can be added to apps.yml when desired.

## File-Manager applicationId

Current Gradle / pairing id: **`ai.victorious.filemanager`** (rename from `ai.victorious.victorfiles` **landed** — [PR #23](https://github.com/Victorious93/File-Manager/pull/23) merged; confirmed in `app/build.gradle.kts`). `apps.yml` reflects the new id.

## Out of scope

VictorLink64 is not part of this fleet index.
