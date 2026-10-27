# Victorious Fleet — APK / Status Roadmap

**As of:** October 27, 2026 (America/New_York)  
**Owner:** Fleet Orchestrator (status docs for this repo)  
**Audience:** Claude Code, Cursor bots, and any agent reading the fleet

This document is **status and planning** only. The live APK update feed remains [`fleet-manifest.json`](./fleet-manifest.json) on GitHub Pages. Do not treat rows here as installable until Don reports phone results.

## Monorepo

**Victorious Fleet monorepo is LIVE:** [Victorious93/victorious-fleet](https://github.com/Victorious93/victorious-fleet) — 23 git submodules + a VictorCam stub (becomes a submodule once `Victorious93/victorcam` main has a commit). Clone with:

```
git clone --recurse-submodules https://github.com/Victorious93/victorious-fleet.git
```

## Global notes

| Note | Detail |
|------|--------|
| Monorepo | [victorious-fleet](https://github.com/Victorious93/victorious-fleet) LIVE — 23 submodules + VictorCam stub. Clone `--recurse-submodules`. |
| GitHub Actions billing | **STILL ACTIVE** (confirmed in the Oct 20 morning health check) — private-repo CI fails. Workaround: make the repo public. Only Don can clear it under Billing & plans. |
| File-Manager | Stacked CI from #26 **RESOLVED** (Oct 25–26). Rename to `ai.victorious.filemanager` landed. Claude rewrote README. |
| README refresh | Oct 20 refresh largely merged. Oct 27 sync: profile + monorepo + ROADMAP + VictorMango/VictorTaskbar + VictorSMS feature README. |
| Merge policy | Auto-merge when CI green + clean (Don, Oct 18, 2026). |
| Secrets | Never commit release signing keys or plaintext signing passwords. Debug keystores are OK. |
| Remote auth | Package id + debug/release cert only. |
| LIVE gate | Don is the real-device tester. Nothing is **LIVE** until he reports phone results. |
| This vs feed | `fleet-manifest.json` = GitHub Pages APK install/update feed; **ROADMAP.md** = status/planning. |

## Status legend

| Status | Meaning |
|--------|---------|
| shipped | In production use / published |
| CI green | Mainline CI passing; may still need device test |
| blocked | Explicit hold (CI stack, policy, or open broken PR) |
| scaffold | Repo/PRs in progress; not fully wired |
| no-repo-yet | Needs empty repo (and Cursor grant where noted) |
| planning | Spec/design; no meaningful CI yet |

---

## CONTROLLER

| App | Package | Repo | Status | Next concrete action | Known blockers |
|-----|---------|------|--------|----------------------|----------------|
| droidcommand-AI | `ai.droidcommand.app` | [Victorious93/droidcommand-AI](https://github.com/Victorious93/droidcommand-AI) (public) | CI green / awaiting device test | Don runs phone tests (a)/(b)/(c) from `docs/P0_P1_HARDWARE_CHECKLIST.md`; first live controller→RootForge edge | None current. Recent merges: #237 bindings, #239 flash catalog, #240 VictorLaunch catalog, #241 P0/P1 RootForge readiness+hardware checklist, #243 VictorLaunch cert rotation **ENFORCED**, #245 VictorSuite TERMUX_IPC transport. #242 Fleet Dashboard home **OPEN** (auto-merge on green). |

---

## BOUND_COMPANION

| App | Package | Repo | Status | Next concrete action | Known blockers |
|-----|---------|------|--------|----------------------|----------------|
| HackerAI-Commander | `ai.hackerai.companion` | [Victorious93/HackerAI-Commander](https://github.com/Victorious93/HackerAI-Commander) | CI green | Wire config into `IHackerAIService` if still pending | None. `config.export`/`import` #21 MERGED. |
| Pentest-Swarm-AI | `ai.pentestswarm.companion` | [Victorious93/Pentest-Swarm-AI](https://github.com/Victorious93/Pentest-Swarm-AI) (public Go) | CI green (APK CI on main) | Keep status/backup cascade current if anything pending | None current. |
| EDL-Commander | `ai.edlcommander.companion` | [Victorious93/EDL-Commander](https://github.com/Victorious93/EDL-Commander) (private Rust) | scaffold / CI | On-device confirm path; Keystore wrapping for backup files | Private repo — Actions billing block **still active**; CI may fail until cleared or repo made public. In-app export with `wrapping:none` refuses Save/Share. #9 MANUAL + #10 envelope+status+backup MERGED. |

---

## EXTERNAL_NODE

| App | Package | Repo | Status | Next concrete action | Known blockers |
|-----|---------|------|--------|----------------------|----------------|
| rootforge-os | `ai.rootforge.os` | [Victorious93/rootforge-os](https://github.com/Victorious93/rootforge-os) (public) | CI green | First live link from controller (Don phone test). P1 first target. | None. #82 `node.status.get` + #83 config backup MERGED. |
| File-Manager | `ai.victorious.filemanager` (rename **LANDED** — confirmed in `app/build.gradle.kts`) | [Victorious93/File-Manager](https://github.com/Victorious93/File-Manager) (private) | CI green (stacked PRs merged) | Pin cert on DCA; device test | **Updated Oct 27:** stacked CI PRs (#23/#25/#26) merged; Claude rewrote the README Oct 25 and fixed split-size/paste/copy bugs through #46. No open PRs. Private repo — Actions billing block may still apply. |
| Victorious-Keyboard | `com.victorious.keyboard` | [Victorious93/Victorious-Keyboard](https://github.com/Victorious93/Victorious-Keyboard) | CI green / merging | Finish remaining media+FM PRs; package rename from florisboard namespace | MANUAL #50 / envelope+status+backup #55 / media+FM-seek #54 on auto-merging path. |
| Victorious Flash / Distro Installer | `com.victorious.flash.android` (desktop) | [Victorious93/Victorious-Flash](https://github.com/Victorious93/Victorious-Flash) (or victorious-flash) | CI green (desktop) | Device/desktop live bridge test | None. FLEET_BRIDGE #18 MERGED; catalogued on DCA as 12th member. |
| Victorious Mesh | `com.victorious.mesh` | [Victorious93/Victorious-Mesh](https://github.com/Victorious93/Victorious-Mesh) | CI green | Live mesh with Browser READ edge (`mesh.status` + `mesh.ip` only) | None. #9 status + #10 UNIFIED_BACKUP MERGED. |
| Victorious Browser | `com.victorious.browser` | [Victorious93/Victorious-Browser](https://github.com/Victorious93/Victorious-Browser) | CI green | Live `mesh.status` / `mesh.ip` path | None. Bridge #24 MERGED; sanctioned node→node READ consumer of Mesh. |
| VictorSMS | `com.victorious.sms` | [Victorious93/victorsms](https://github.com/Victorious93/victorsms) (**now PUBLIC**) | CI green / feature-rich scaffold (not device-tested) | Pin debug cert on DCA; Don device test | **Updated Oct 27:** PRs #1–#12 MERGED. Code has Matrix shell, native SMS + unified inbox, phone dialer (`ROLE_DIALER` + InCallService), spam call/text blocking, Twilio alias number (SIP stub), persona drafter + voice notes (PC LLM/TTS over mesh), summarizer/triage + DCA fleet-command bot, SMS remote-command trigger (forward-only, never executes), ABI splits + phone-to-phone relay, fleet bridge. README still under-describes this — sync PR opening. |
| VictorLaunch | `com.victorious.launch` | [Victorious93/VictorLaunch](https://github.com/Victorious93/VictorLaunch) | CI green | Public repo finalize; Don install | NEW cert `B9:9A:AF:…:4A:69` enforced on DCA #243; old `1B:06` revoked. #2 + #6 (debug key rotation) MERGED; going public. Full-control exception already Don-approved. |
| VictorSpoof | `com.victorious.spoof` | [Victorious93/victorspoof](https://github.com/Victorious93/victorspoof) | scaffold | Merge #1; `config.export`/`import` follow-up; release cert TBD | PR **#1 OPEN** (billing block still active for private repos — recheck CI). Debug cert `A4:84:…:FF:EE`. |
| VictorLink64 | `org.victoriousframework.victorlink64` | [Victorious93/VictorLink64](https://github.com/Victorious93/VictorLink64) | early beta / public | Product work (not fleet bridge) | **NOT_IN_FLEET_REMOTE** — game emulator; not remote-control fleet. |
| VictorCam | `com.victorious.cam` | [Victorious93/victorcam](https://github.com/Victorious93/victorcam) (public; was empty, scaffold pushing) | scaffold | Land scaffold on main; then swap the monorepo stub for a real submodule in victorious-fleet; onboard EXTERNAL_NODE standards + debug cert | Logo `victorcam-logo.jpg` exists in fleet-logos. OBS-style virtual cam. |
| VictorMango | `com.victorious.mango` | [Victorious93/victormango](https://github.com/Victorious93/victormango) (public) | scaffold | Implement stubbed router features (Wi-Fi/VPN/AdGuard/Tether/Firmware); add to monorepo as submodule | **NEW (Oct 27 sync):** Android companion for GL.iNet **Mango (GL-MT300N-V2)** / OpenWrt travel routers. GL.iNet 4.x `/rpc` auth is real; feature calls are stubbed. Debug keystore committed (OK). Not yet in `apps.yml` feed until it ships an APK. |
| VictorTaskbar | `com.victorious.taskbar` | [Victorious93/victortaskbar](https://github.com/Victorious93/victortaskbar) (public) | planning (repo just created) | Scaffold the app; onboard fleet standards | **NEW (Oct 27 sync):** desktop-style taskbar for Android. README-only initial commit so far — no app build yet. |

---

## TERMUX_IPC / HOST_PROCESS_TOOL

| App | Package | Repo | Connection | Status | Next concrete action | Known blockers |
|-----|---------|------|------------|--------|----------------------|----------------|
| VictorSuite | `com.termux` | [Victorious93/VictorSuite](https://github.com/Victorious93/VictorSuite) | TERMUX_IPC | CI green / building — not device-tested | Land the **AI Coding Agents** pack + confirm-flow #49; Don device test for TERMUX_IPC still pending | #48 envelope v1 MERGED; DCA #245 transport MERGED. AI Coding Agents pack in progress: one-tap Claude Code, Codex CLI, Ollama, Copilot CLI in Termux; root/Shizuku phone-control bridge with approval gates; token-minimizer; automatic per-task model switching. Phone-to-PC offload routing: when/how/where heavy AI work goes to Don's PC (wake via Pyra Claw), with fallback to on-phone or hosted if the PC is unavailable. |
| KYC-AuditLab / IdentityShield | `com.identityshield.companion` | [Victorious93/KYC-AuditLab](https://github.com/Victorious93/KYC-AuditLab) | (hold) | blocked (policy hold) | Wait Don explicit OK in KYC's **own** chat before any remote surface | HOLD remote-control surface + merges until Don OK. |
| BakerPi-Installer | n/a (host tool) | [Victorious93/BakerPi-Installer](https://github.com/Victorious93/BakerPi-Installer) (public) | HOST_PROCESS_TOOL | CI green | Keep installer current | None. #13 MANUAL MERGED. |

---

## HARDWARE

| App | Package | Repo | Connection | Status | Next concrete action | Known blockers |
|-----|---------|------|------------|--------|----------------------|----------------|
| Pyra Claw | n/a (hardware) | [Victorious93/pyra-claw](https://github.com/Victorious93/pyra-claw) (submodule in victorious-fleet) | HARDWARE → fold to EXTERNAL_NODE | planning / scaffold | Fold into fleet standards; EXTERNAL_NODE contract | Pi5+ESP32 hub. |
| VictorModem | n/a (hardware; no package yet) | [Victorious93/victormodem](https://github.com/Victorious93/victormodem) (private submodule in victorious-fleet) | HARDWARE | planning / scaffold | Onboard to fleet standards | Private — may be billing-blocked. ESP32-S3 + cellular. Logo exists in fleet-logos. |

---

## SOCIAL

| App | Package | Repo | Connection | Status | Next concrete action | Known blockers |
|-----|---------|------|------------|--------|----------------------|----------------|
| MatchVictorious | `com.victorious.match` | [Victorious93/matchvictorious](https://github.com/Victorious93/matchvictorious) (submodule in victorious-fleet) | SOCIAL | scaffold / planning | Onboard to fleet standards | Dating hub. Logo in fleet-logos. |
| VictoryPed | `com.victorious.victoryped` | [Victorious93/VictoryPed](https://github.com/Victorious93/VictoryPed) (private submodule in victorious-fleet) | SOCIAL | scaffold / planning | Onboard to fleet standards | Private — may be billing-blocked. Bully breeds. Logo in fleet-logos (`victorpedigree-*`). |

---

## GAMES (not in fleet remote)

| App | Package | Repo | Connection | Status | Next concrete action | Known blockers |
|-----|---------|------|------------|--------|----------------------|----------------|
| VictorOps Zombies | `com.victorious.zombie` | [Victorious93/victorops-zombies](https://github.com/Victorious93/victorops-zombies) (**now PUBLIC**) | NOT_IN_FLEET_REMOTE (game; libGDX+Kotlin) | scaffold (playable slice) | Device test; keep in sync with VictorLink64 built-in version | **Updated Oct 27:** repo is public; scaffold PRs #1–#4 merged (characters, tiered gun upgrades, armor/ammo, TCP hardening + Fortify cooldown fix). Also shipped as a built-in game inside VictorLink64. targetSdk 35 / minSdk 24. |

---

## Quick matrix (connection class)

| Connection class | Members |
|------------------|---------|
| CONTROLLER | droidcommand-AI |
| BOUND_COMPANION | HackerAI-Commander, Pentest-Swarm-AI, EDL-Commander |
| EXTERNAL_NODE | rootforge-os, File-Manager, Victorious-Keyboard, Victorious Flash, Victorious Mesh, Victorious Browser, VictorSMS, VictorLaunch, VictorSpoof, VictorCam, VictorMango, VictorTaskbar (planning) |
| NOT_IN_FLEET_REMOTE | VictorLink64, VictorOps Zombies |
| TERMUX_IPC | VictorSuite |
| HOST_PROCESS_TOOL | BakerPi-Installer |
| HARDWARE | Pyra Claw, VictorModem |
| SOCIAL | MatchVictorious, VictoryPed |
| Policy hold | KYC-AuditLab / IdentityShield |

---

*Generated for Fleet Orchestrator · Oct 20, 2026 · README/status sync Oct 27, 2026 · Do not merge release keys. Don phone reports gate LIVE.*
