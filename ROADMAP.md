# Victorious Fleet — APK / Status Roadmap

**As of:** October 20, 2026 (America/New_York)  
**Owner:** Fleet Orchestrator (status docs for this repo)  
**Audience:** Claude Code, Cursor bots, and any agent reading the fleet

This document is **status and planning** only. The live APK update feed remains [`fleet-manifest.json`](./fleet-manifest.json) on GitHub Pages. Do not treat rows here as installable until Don reports phone results.

## Global notes

| Note | Detail |
|------|--------|
| GitHub Actions billing | **RESOLVED** — private CI was previously blocked by a $0 Actions cap; cleared. Do not list billing as a current blocker unless a specific app is still stuck. |
| File-Manager | Stacked CI failures being fixed at **#26+** (stack on #25 → #23). |
| Merge policy | Auto-merge when CI green + clean (Don, Oct 18, 2026). |
| Secrets | Never commit release signing keys or plaintext signing passwords. |
| Remote auth | Package id + debug/release cert only. |
| LIVE gate | Don is the real-device tester. Nothing is **LIVE** until he reports phone results. |
| This vs feed | `fleet-manifest.json` = install/update feed; **ROADMAP.md** = status/planning. |

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
| Pentest-Swarm-AI | `ai.pentestswarm.companion` | [Victorious93/Pentest-Swarm-AI](https://github.com/Victorious93/Pentest-Swarm-AI) (public Go) | CI green [Likely] | Status/backup cascade if not done; confirm MANUAL | Confirm CI/MANUAL state on main if unsure. |
| EDL-Commander | `ai.edlcommander.companion` | [Victorious93/EDL-Commander](https://github.com/Victorious93/EDL-Commander) (private Rust) | scaffold / CI | On-device confirm path; Keystore wrapping for backup files | Historically billing-blocked; **billing RESOLVED**. In-app export with `wrapping:none` refuses Save/Share. #9 MANUAL + #10 envelope+status+backup MERGED. |

---

## EXTERNAL_NODE

| App | Package | Repo | Status | Next concrete action | Known blockers |
|-----|---------|------|--------|----------------------|----------------|
| rootforge-os | `ai.rootforge.os` | [Victorious93/rootforge-os](https://github.com/Victorious93/rootforge-os) (public) | CI green | First live link from controller (Don phone test). P1 first target. | None. #82 `node.status.get` + #83 config backup MERGED. |
| File-Manager | `ai.victorious.filemanager` (target; repo may still say `ai.victorious.victorfiles`) | [Victorious93/File-Manager](https://github.com/Victorious93/File-Manager) (private) | blocked (CI stack) | Unblock #26 stack; finish rename to `ai.victorious.filemanager`; pin on DCA | PR **#26 OPEN** stacked on #25→#23 — stacked CI failures being fixed. |
| Victorious-Keyboard | `com.victorious.keyboard` | [Victorious93/Victorious-Keyboard](https://github.com/Victorious93/Victorious-Keyboard) | CI green / merging | Finish remaining media+FM PRs; package rename from florisboard namespace | MANUAL #50 / envelope+status+backup #55 / media+FM-seek #54 on auto-merging path. |
| Victorious Flash / Distro Installer | `com.victorious.flash.android` (desktop) | [Victorious93/Victorious-Flash](https://github.com/Victorious93/Victorious-Flash) (or victorious-flash) | CI green (desktop) | Device/desktop live bridge test | None. FLEET_BRIDGE #18 MERGED; catalogued on DCA as 12th member. |
| Victorious Mesh | `com.victorious.mesh` | [Victorious93/Victorious-Mesh](https://github.com/Victorious93/Victorious-Mesh) | CI green | Live mesh with Browser READ edge (`mesh.status` + `mesh.ip` only) | None. #9 status + #10 UNIFIED_BACKUP MERGED. |
| Victorious Browser | `com.victorious.browser` | [Victorious93/Victorious-Browser](https://github.com/Victorious93/Victorious-Browser) | CI green | Live `mesh.status` / `mesh.ip` path | None. Bridge #24 MERGED; sanctioned node→node READ consumer of Mesh. |
| VictorSMS | `com.victorious.sms` | [Victorious93/VictorSMS](https://github.com/Victorious93/VictorSMS) (or victorsms) | scaffold / blocked | Land #1; relay debug cert if not yet pinned | PR **#1 OPEN** (compileSdk 37 fix in progress). |
| VictorLaunch | `com.victorious.launch` | [Victorious93/VictorLaunch](https://github.com/Victorious93/VictorLaunch) | CI green | Public repo finalize; Don install | NEW cert `B9:9A:AF:…:4A:69` enforced on DCA #243; old `1B:06` revoked. #2 + #6 (debug key rotation) MERGED; going public. Full-control exception already Don-approved. |
| VictorSpoof | `com.victorious.spoof` | [Victorious93/victorspoof](https://github.com/Victorious93/victorspoof) | scaffold | Merge #1; `config.export`/`import` follow-up; release cert TBD | PR **#1 OPEN** (was billing-blocked; billing resolved — recheck CI). Debug cert `A4:84:…:FF:EE`. |
| VictorLink64 | `org.victoriousframework.victorlink64` | [Victorious93/VictorLink64](https://github.com/Victorious93/VictorLink64) | early beta / public | Product work (not fleet bridge) | **NOT_IN_FLEET_REMOTE** — game emulator; not remote-control fleet. |
| VictorCam | `com.victorious.cam` | not-yet-created / confirm with bot | scaffold / planning | Onboard to EXTERNAL_NODE standards + debug cert; confirm repo name with bot | Logo `victorcam-logo.jpg` exists in fleet-logos. OBS-style virtual cam. |

---

## TERMUX_IPC / HOST_PROCESS_TOOL

| App | Package | Repo | Connection | Status | Next concrete action | Known blockers |
|-----|---------|------|------------|--------|----------------------|----------------|
| VictorSuite | `com.termux` | [Victorious93/VictorSuite](https://github.com/Victorious93/VictorSuite) | TERMUX_IPC | CI green / not device-tested | Don device test; on-device confirm (#49) | #48 envelope v1 MERGED; DCA #245 transport MERGED. Confirm-flow issue #49. |
| KYC-AuditLab / IdentityShield | `com.identityshield.companion` | [Victorious93/KYC-AuditLab](https://github.com/Victorious93/KYC-AuditLab) | (hold) | blocked (policy hold) | Wait Don explicit OK in KYC's **own** chat before any remote surface | HOLD remote-control surface + merges until Don OK. |
| BakerPi-Installer | n/a (host tool) | [Victorious93/BakerPi-Installer](https://github.com/Victorious93/BakerPi-Installer) (public) | HOST_PROCESS_TOOL | CI green | Keep installer current | None. #13 MANUAL MERGED. |

---

## HARDWARE

| App | Package | Repo | Connection | Status | Next concrete action | Known blockers |
|-----|---------|------|------------|--------|----------------------|----------------|
| Pyra Claw | n/a (hardware) | pyra-claw / Victorious93 TBD | HARDWARE → fold to EXTERNAL_NODE | planning / scaffold | Fold into fleet standards; EXTERNAL_NODE contract | Pi5+ESP32 hub. |
| VictorModem | n/a (hardware; no package yet) | no-repo-yet | HARDWARE | no-repo-yet / planning | Create repo when Don ready | ESP32-S3 + cellular. Logo exists in fleet-logos. |

---

## SOCIAL

| App | Package | Repo | Connection | Status | Next concrete action | Known blockers |
|-----|---------|------|------------|--------|----------------------|----------------|
| MatchVictorious | `com.victorious.match` | not-yet-confirmed | SOCIAL | scaffold / planning | Confirm repo + onboard | Dating hub. Logo in fleet-logos. |
| VictoryPed | `com.victorious.victoryped` | not-yet-confirmed | SOCIAL | scaffold / planning | Confirm repo + onboard | Bully breeds. Logo in fleet-logos (`victorpedigree-*`). |

---

## NEW (not fully rostered yet)

| App | Package | Repo | Connection | Status | Next concrete action | Known blockers |
|-----|---------|------|------------|--------|----------------------|----------------|
| VictorOps Zombies | `com.victorious.zombie` | no-repo-yet | NOT_IN_FLEET_REMOTE (game; libGDX+Kotlin) | no-repo-yet | GitHub bot creates empty **private** `Victorious93` repo + Cursor grant for **VictorOps Zombies** bot | Bot id exists: VictorOps Zombies. Logo `victorops-zombies-logo.jpg` in fleet-logos. |

---

## Quick matrix (connection class)

| Connection class | Members |
|------------------|---------|
| CONTROLLER | droidcommand-AI |
| BOUND_COMPANION | HackerAI-Commander, Pentest-Swarm-AI, EDL-Commander |
| EXTERNAL_NODE | rootforge-os, File-Manager, Victorious-Keyboard, Victorious Flash, Victorious Mesh, Victorious Browser, VictorSMS, VictorLaunch, VictorSpoof, VictorCam |
| NOT_IN_FLEET_REMOTE | VictorLink64, VictorOps Zombies |
| TERMUX_IPC | VictorSuite |
| HOST_PROCESS_TOOL | BakerPi-Installer |
| HARDWARE | Pyra Claw, VictorModem |
| SOCIAL | MatchVictorious, VictoryPed |
| Policy hold | KYC-AuditLab / IdentityShield |

---

*Generated for Fleet Orchestrator · Oct 20, 2026 · Do not merge release keys. Don phone reports gate LIVE.*
