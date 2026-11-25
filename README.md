<div align="center">

<img src="assets/logo.jpg" width="200" alt="fleet-manifest logo"/>

# fleet-manifest

**Fleet APK update feed, trust pins and status docs for the Victorious fleet.**

<p>
  <a href="https://github.com/Victorious93/fleet-manifest/actions/workflows/update-manifest.yml"><img src="https://img.shields.io/github/actions/workflow/status/Victorious93/fleet-manifest/update-manifest.yml?branch=main&style=flat-square&labelColor=0B0F1A&logo=githubactions&logoColor=00E5FF&label=CI" alt="CI"/></a>
  <img src="https://img.shields.io/badge/platform-JSON%20feed-00E5FF?style=flat-square&labelColor=0B0F1A" alt="Platform"/>
  <a href="https://victorious-framework.pages.dev"><img src="https://img.shields.io/badge/Victorious-Framework-FF2BD6?style=flat-square&labelColor=0B0F1A" alt="Victorious Framework"/></a>
</p>

<p><a href="https://victorious-framework.pages.dev">Victorious Framework</a></p>

</div>

---

Fleet APK update feed and status/planning docs for the Victorious fleet. There is no app to build: the authoritative feed is [`fleet-manifest.json`](fleet-manifest.json) and the status board is [`ROADMAP.md`](ROADMAP.md).

<p align="center"><a href="https://victorious93.github.io/fleet-manifest/"><strong>Home</strong></a> | <a href="https://victorious93.github.io/fleet-manifest/shop/"><strong>Shop</strong></a></p>

🛍️ **[ShopVictorious](https://victorious93.github.io/fleet-manifest/shop/)**: Victorious Apparel merch (buy links coming soon).

## What it is

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

## Use it

Read the live feed at the URL above (or the raw fallback). Clients must apply the trust model before trusting any entry. Feed updates are committed automatically by the `update-manifest` workflow, so edit `apps.yml` (not `fleet-manifest.json`) to change which apps are covered.

---

## Victorious fleet

fleet-manifest is part of the [Victorious Framework](https://victorious-framework.pages.dev), a family of Android and PC tools by Don Victor. **Every app works standalone; fleet integrations are optional** and never required.

Related: [victorious-fleet](https://github.com/Victorious93/victorious-fleet) (index of all fleet apps), [droidcommand-AI](https://github.com/Victorious93/droidcommand-AI) (fleet controller), [VictorLaunch](https://github.com/Victorious93/victorlaunch) (fleet launcher and theme provider). The full roster lives in [victorious-fleet](https://github.com/Victorious93/victorious-fleet), and live status is in [fleet-manifest](https://github.com/Victorious93/fleet-manifest).

## Contributing

Issues and pull requests are welcome. Keep changes focused, match the existing code style, never commit secrets or release keys, and keep any fleet integration optional so the app still works on its own. Open a PR against the default branch and wait for CI to pass.

## License

This repository does not include a LICENSE file yet, so no license is granted beyond what the author states elsewhere. Shop images are the owner's artwork; please open an issue before reusing them.
