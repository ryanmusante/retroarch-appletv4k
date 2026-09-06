# retroarch-appletv4k

[![version](https://img.shields.io/badge/version-5.4-blue.svg)](CHANGELOG.md)
[![companion](https://img.shields.io/badge/companion-retroarch--configs-blue.svg)](https://github.com/ryanmusante/retroarch-configs)

> RetroArch setup for Apple TV 4K 3rd Gen (tvOS 26, RetroArch v1.22.x)
> with a 74-key `retroarch.cfg`. Companion to
> [retroarch-configs](https://github.com/ryanmusante/retroarch-configs),
> which ships the per-core `.cfg` / `.opt` files.

## Quick Start

1. App Store → RetroArch (developer: Libretro). On first launch note the two URLs in the Welcome popup; the IP is also under tvOS Settings → Network.
2. Main Menu → Online Updater → Assets, Core Info Files, Databases, Slang Shaders.
3. Upload `retroarch.cfg` to `/config/` ([File Transfers](#file-transfers)), then quit and relaunch RetroArch — the menu should come back in XMB's Gray Dark theme.
4. Pair a controller ([Controllers](#controllers)), bind the hotkeys, set the two directories ([Storage Persistence](#storage-persistence)) and save the configuration once ([Configuration](#configuration)).
5. Upload ROMs and BIOS ([Systems](#systems)); Main Menu → Import Content → Manual Scan per system folder ([Manual Scan](#manual-scan)). Playlists appear as XMB tabs.

> [!IMPORTANT]
> Do not use "Save Current Configuration" between uploading
> `retroarch.cfg` and relaunching — it overwrites the uploaded file with
> the in-memory config.

<details open>
<summary><b>Prerequisites</b></summary>

| Component | Requirement |
|-----------|-------------|
| Apple TV | 4K 3rd Gen (A2737, A15 Bionic), 64 GB Wi-Fi; tvOS 26 target (App Store floor tvOS 13) |
| RetroArch | ≥ v1.20.0 (WebDAV); v1.22.x recommended |
| Controller | PS5 DualSense or Xbox Series X/S; the Siri Remote is menu-only |
| Network | Apple TV and computer on the same LAN (the 64 GB model has no Ethernet) |
| BIOS | Sega CD, TurboGrafx-CD, Neo Geo ([Systems](#systems)) |

</details>

## Storage Persistence

> [!IMPORTANT]
> tvOS guarantees only 500 KB of persistent storage per app. Everything
> under the web root — ROMs, BIOS, saves, states, playlists, shaders and
> per-core overrides — lives in purgeable cache that tvOS deletes silently
> under storage pressure. Back it up via WebDAV.

`retroarch.cfg` is mirrored to NSUserDefaults (RetroArch ≥ 1.16.0) and
restored from there after a purge, with assets re-extracted (≥ 1.18.0).
The mirror is refreshed only by Save Current Configuration, so an
uploaded file is unprotected until saved once. After the uploaded file
has loaded, set Settings → Directory → File Browser to `config/ROMs/` and
System/BIOS to `config/BIOS/`, then save — both paths ride along in the
mirror.

## File Transfers

tvOS has no Files app; RetroArch's built-in servers handle all transfers
and RetroArch must be running.

<details open>
<summary><b>Servers</b></summary>

| Server | Port | Endpoint |
|--------|------|----------|
| Web interface | 80 | `http://<atv-ip>` or `http://<device-name>.local` (e.g. `appletv.local`) |
| WebDAV (v1.20.0+) | 8080 | `http://<atv-ip>:8080` (Finder ⌘K / Map Network Drive) |

</details>

From Linux: Dolphin `webdav://<atv-ip>:8080`, GNOME Files
`dav://<atv-ip>:8080`, or
`curl -T retroarch.cfg http://<atv-ip>:8080/config/retroarch.cfg`.

> [!IMPORTANT]
> Both servers are unauthenticated with no auth or TLS option — anyone on
> the LAN can read or overwrite saves, states and configuration.
> Restrict ports 80 and 8080 on the Apple TV's IP with router firewall
> rules or a VLAN.

```
/                              ← web interface / WebDAV root (tvOS cache)
├── config/
│   ├── retroarch.cfg          ← this repo
│   ├── ROMs/<system>/         ← see Systems (File Browser directory)
│   ├── BIOS/                  ← case-sensitive filenames (System/BIOS directory)
│   └── <Core Name>/           ← per-core .cfg + .opt (retroarch-configs)
├── saves/  states/            ← per-core subfolders (sort_savefiles_enable / sort_savestates_enable)
├── playlists/                 ← Manual Scan output (.lpl)
├── screenshots/               ← GPU screenshots
└── shaders/shaders_slang/     ← Online Updater → Update Slang Shaders
```

## Systems

Tier 1 = full speed with shaders; Tier 2 = most titles at full speed.
JIT-required systems (Dreamcast, GameCube, Wii, PS2) are not supported
on the App Store build. Folder names are a convention — Manual Scan
matches against the system database you select.

<details open>
<summary><b>System table</b></summary>

| System | Tier | Folder | Extensions | Core | System Name (Manual Scan) | BIOS (`config/BIOS/`) |
|--------|------|--------|------------|------|---------------------------|-----------------------|
| NES | 1 | `nes/` | `.nes` `.unf` `.unif` | Mesen | `Nintendo - Nintendo Entertainment System` | — |
| SNES | 1 | `snes/` | `.sfc` `.smc` | Snes9x | `Nintendo - Super Nintendo Entertainment System` | — |
| GB / GBC / GBA | 1 | `gb/` `gbc/` `gba/` | `.gb` `.gbc` `.gba` | mGBA | `Nintendo - Game Boy`; `Nintendo - Game Boy Color`; `Nintendo - Game Boy Advance` | `gba_bios.bin` (optional) |
| Genesis / MD, Master System | 1 | `megadrive/` `mastersystem/` | `.md` `.gen` `.bin`; `.sms` | Genesis Plus GX | `Sega - Mega Drive - Genesis`; `Sega - Master System - Mark III` | — |
| Sega CD / Mega CD | 1 | `segacd/` | `.cue` `.chd` | Genesis Plus GX | `Sega - Mega-CD - Sega CD` | `bios_CD_U.bin`, `bios_CD_E.bin`, `bios_CD_J.bin` |
| PC Engine / TG-16, TurboGrafx-CD | 1 | `pce/` | `.pce`; `.cue` `.chd` | Beetle PCE Fast | `NEC - PC Engine - TurboGrafx 16`; `NEC - PC Engine CD - TurboGrafx-CD` | `syscard3.pce` (CD) |
| Neo Geo, Arcade (CPS1/2/3) | 1 | `neogeo/` `fbneo/` | `.zip` | FinalBurn Neo | `SNK - Neo Geo`; `FBNeo - Arcade Games` | `neogeo.zip` (also in `config/ROMs/neogeo/`) |
| Nintendo 64 | 2 | `n64/` | `.n64` `.z64` `.v64` | Mupen64Plus-Next | `Nintendo - Nintendo 64` | — |

</details>

### Manual Scan

One scan per System Name: Content Directory → the folder, System Name →
the database above, Default Core → the core, Start Scan. Where one
folder holds two systems (`pce/`), set File Extensions (`pce` vs
`cue chd`) to split the scans. Arcade: Scan Inside Archives off (the
`.zip` is the ROM); Arcade DAT File → `FinalBurn Neo (ClrMame Pro XML,
Arcade only).dat` from
[FBNeo/dats](https://github.com/libretro/FBNeo/tree/master/dats),
uploaded to the device, gives proper titles, and Arcade DAT Filter drops
sets the core cannot load — FBNeo wants sets built against its own DAT,
not MAME's. Prefer `.chd` over `.cue` + `.bin` for CD images.

Per-core overrides and core options for these cores ship in
[retroarch-configs](https://github.com/ryanmusante/retroarch-configs).

## Controllers

Pair via tvOS Settings → Remotes and Devices → Bluetooth. RetroArch on
tvOS recognizes at most three controllers
([#16685](https://github.com/libretro/RetroArch/issues/16685)); ghost
inputs from controllers 2+ can bleed into controller 1
([#18447](https://github.com/libretro/RetroArch/issues/18447)). Backing
out of the menu root backgrounds the app on any pad — tvOS behaviour,
not a bug ([#18286](https://github.com/libretro/RetroArch/issues/18286)).

<details open>
<summary><b>Compatibility</b></summary>

| Controller | Status |
|------------|--------|
| PS5 DualSense / Edge, Xbox Series X/S | Recommended |
| PS4 DualShock 4, 8BitDo Pro 2, SteelSeries Nimbus+ | Works |
| Nintendo Switch Pro | Caution — source of the [#18286](https://github.com/libretro/RetroArch/issues/18286) exit report (root-menu Back, see above); A / B labels sit swapped against the PS / Xbox layout |

</details>

## Configuration

tvOS reserves the controller Home button (PS / Xbox) for the system —
RetroArch never sees it, so the mFi autoconfig's Home-button Menu Toggle
never fires. `retroarch.cfg` pins the Menu Toggle combo to L3 + R3
(`input_menu_toggle_gamepad_combo = "2"`; the tvOS default is Down + Y +
L1 + R1) — the only way back to the menu short of force-quitting, so
keep it. No other gamepad hotkey is bound: under Settings → Input →
Hotkeys bind Hotkey Enable → Select first, then the table below, and
persist them with Main Menu → Configuration File → Save Current
Configuration (safe once the uploaded file has loaded; the save also
refreshes the NSUserDefaults mirror, and `config_save_on_exit = "false"`
would otherwise drop the binds on quit). Game Focus must stay off
(`input_auto_game_focus = "0"`, shipped) — it blocks every hotkey while
content runs. Re-uploading the 74-key `retroarch.cfg` later replaces the
saved file, so re-apply the binds and directories and save again.

<details open>
<summary><b>Hotkeys</b></summary>

| Action | Combo |
|--------|-------|
| Quick Menu | L3 + R3 |
| Save / Load State | Select + R1 / Select + L1 |
| State Slot ± | Select + D-Pad Right / Left |
| Fast Forward / Rewind | Select + R2 / Select + L2 (rewind off globally; enable per-game) |
| Close Content | Select + Start (auto-saves to slot Auto; to resume, step State Slot down to Auto, then Select + L1) |

</details>

SRAM flushes every 5 min (`autosave_interval = "300"`; Mupen pins `0`)
and is protected from state-load overwrite. Close Content writes one
auto-save state to slot Auto (`.state.auto`); nothing auto-loads, so step
the State Slot down to Auto and Load State. Manual saves auto-increment
the slot, keeping at most 10.

<details open>
<summary><b>What <code>retroarch.cfg</code> sets</b></summary>

| Section | Keys | Sets |
|---------|------|------|
| Menu / UI | 18 | XMB (theme 20 Gray Dark, no shadows) with widgets; sublabels, horizontal and load-content animations off, XMB animation styles pinned, background pipeline off; favorites and history capped at 10; playlist compression and core-info cache on; no config save on exit; no pause when tvOS marks the app inactive; emulation pauses in the menu |
| Video | 14 | `metal` driver; integer scaling with core-provided aspect; no bilinear smoothing; shaders on; vsync; auto frame delay; threaded video off; 60 Hz; FPS counter; GPU screenshots; HDR off (1000-nit ceiling); screensaver suppressed |
| Audio | 5 | `coreaudio` at 48 kHz; audio sync; resampler quality 3; 64 ms latency |
| Latency / Run Ahead | 7 | VRR runloop off; run-ahead off globally (2 frames, single instance, warnings hidden — Tier 1 cores enable it per-core); preemptive frames off (mutually exclusive with run-ahead); fast-forward 4× |
| Input | 5 | L3 + R3 menu combo; `mfi` joypad driver; Game Focus off; overlays off; 4 users |
| Saves / Savestates | 11 | compressed saves and states sorted per core; 5-min SRAM flush protected from state-load overwrite; auto-save state to slot Auto on Close Content; auto-indexed manual slots, at most 10 kept, no thumbnails; rewind off |
| Security | 4 | network commands (interface exposed on tvOS since 1.22.1), network remote, on-demand thumbnails and cloud sync off |
| Null drivers | 6 | bluetooth, wifi, midi, record, camera, location |
| Netplay | 3 | public announce, NAT traversal and MITM relay off |
| Logging | 1 | verbosity off |

</details>

<details open>
<summary><b>Tuning</b></summary>

| Key | Shipped | Change when |
|-----|---------|-------------|
| `video_hdr_enable` | `false` | Never on v1.22.x — the Metal driver has no HDR output until master, where the key becomes `video_hdr_mode` |
| `audio_latency` | `64` | `48` for lower latency if audio stays clean |
| `fastforward_ratio` | `4.0` | Raise only with thermal headroom (passive A15) |
| `fps_show` | `true` | `false` to hide the on-screen frame-rate counter |
| `video_refresh_rate` | `60.000000` | Seed for 60 Hz SDR; calibrate via Settings → Video → Output |
| `vrr_runloop_enable` | `false` | Never — Apple TV has no game VRR; `true` disables Dynamic Rate Control (judder, desync) |
| `run_ahead_enabled` | `false` | Per-core `true` on Tier 1 via retroarch-configs; per-game on Mupen |

</details>

<details open>
<summary><b>TV output</b></summary>

| Setting | Location | Value |
|---------|----------|-------|
| Resolution | tvOS Settings → Video and Audio | 4K SDR 60 Hz |
| Match Frame Rate / Dynamic Range | tvOS Settings → Video and Audio → Match Content | Off / Off |
| Audio Format | tvOS Settings → Video and Audio | Stereo |
| Reduce Loud Sounds | tvOS Settings → Video and Audio | Off |
| Game Mode | TV (HDMI input) | On |
| Chroma subsampling | TV (HDMI input) | 4:4:4 or RGB Full |

</details>

## Shaders

The pipeline is on (`video_shader_enable = "true"`) with no global
preset. Per core: Quick Menu → Shaders → Load Preset → pick a preset →
Manage Presets → Save Core Preset; adjust preset parameters under Quick
Menu → Shaders → Shader Parameters; Manage Presets → Remove Core Preset
undoes it. Avoid CRT-Royale, CRT-Geom-Deluxe,
Guest-Dr-Venom, Guest-Advanced and Mega Bezel — they exceed the A15 GPU
budget.

<details open>
<summary><b>Presets</b></summary>

| Shader | Use for |
|--------|---------|
| `crt/zfast-crt.slangp` | All systems; single-pass, integer-scale safe |
| `handheld/lcd-grid-v2.slangp` | mGBA (GB / GBC / GBA) for an LCD look |

</details>

## Troubleshooting

<details open>
<summary><b>Symptoms</b></summary>

| Symptom | Fix |
|---------|-----|
| Uploaded `retroarch.cfg` changed nothing | It must sit at `/config/retroarch.cfg`; relaunch without saving first ([File Transfers](#file-transfers)) |
| Pad cannot reach the menu | L3 + R3 only — tvOS keeps the Home button ([Configuration](#configuration)) |
| Hotkeys dead while content runs | Game Focus is on: keep `input_auto_game_focus = "0"` and leave its toggle unbound ([Configuration](#configuration)) |
| App drops to the tvOS Home Screen | Back at the menu root — tvOS behaviour on any pad ([Controllers](#controllers)) |
| Fourth pad ignored, phantom inputs on pad 1 | Three-pad cap and #18447 ghosting ([Controllers](#controllers)) |
| ROMs, saves or overrides vanished | tvOS purged the cache: config returns from the mirror if saved once, assets re-extract, re-upload the rest ([Storage Persistence](#storage-persistence)) |
| Arcade set will not load | Set must match FBNeo's DAT; `neogeo.zip` beside Neo Geo games ([Manual Scan](#manual-scan)) |
| Audio crackle | Raise `audio_latency` (`64` → `96`) before touching anything else |
| Judder or drift | Match Frame Rate off, 4K SDR 60 Hz, `vrr_runloop_enable` stays `false` ([Configuration](#configuration)) |

</details>

## Versioning

`vMAJOR.MINOR`, in lockstep with `retroarch-configs` — one tag per
release across both repos. `MAJOR` on incompatible structural changes,
`MINOR` otherwise. `CHANGELOG.md` retains the last 5 releases.

## License

MIT © 2026 Ryan Musante. RetroArch is a separate project under GPL v3;
this guide and configuration are independent community resources.
