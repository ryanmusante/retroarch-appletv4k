# retroarch-appletv4k

[![version](https://img.shields.io/badge/version-5.0-blue.svg)](CHANGELOG.md)
[![companion](https://img.shields.io/badge/companion-retroarch--configs-blue.svg)](https://github.com/ryanmusante/retroarch-configs)

> RetroArch setup for Apple TV 4K 3rd Gen (tvOS 26, RetroArch v1.22.x)
> with a 74-key `retroarch.cfg`. Companion to
> [retroarch-configs](https://github.com/ryanmusante/retroarch-configs),
> which ships the per-core `.cfg` / `.opt` files.

## Quick Start

1. App Store → RetroArch (developer: Libretro). On first launch note the two URLs in the Welcome popup.
2. Main Menu → Online Updater → Assets, Core Info Files, Databases, Slang Shaders.
3. Upload `retroarch.cfg` to `/` ([File Transfers](#file-transfers)), then quit and relaunch RetroArch.
4. Pair a controller ([Controllers](#controllers)) and set the Menu Toggle combo ([Configuration](#configuration)).
5. Upload ROMs and BIOS ([Systems](#systems)); Main Menu → Import Content → Manual Scan per system folder.

> [!IMPORTANT]
> Do not use "Save Current Configuration" between uploading
> `retroarch.cfg` and relaunching — it overwrites the uploaded file with
> the in-memory config.

<details open>
<summary><b>Prerequisites</b></summary>

| Component | Requirement |
|-----------|-------------|
| Apple TV | 4K 3rd Gen (A2737, A15 Bionic), 64 GB Wi-Fi, tvOS 26 |
| RetroArch | ≥ v1.20.0 (WebDAV); v1.22.x recommended |
| Controller | PS5 DualSense or Xbox Series X/S; the Siri Remote is menu-only |
| Network | Apple TV and computer on the same LAN (the 64 GB model has no Ethernet) |
| BIOS | Sega CD, TurboGrafx-CD, Neo Geo ([Systems](#systems)) |

</details>

## Storage Persistence

> [!IMPORTANT]
> tvOS guarantees only 500 KB of persistent storage per app. ROMs, saves,
> states, BIOS and shaders live in purgeable cache that tvOS deletes
> silently under storage pressure — back them up via WebDAV.

`retroarch.cfg` is mirrored to NSUserDefaults (RetroArch ≥ 1.16.0) and
assets re-extract after a purge (≥ 1.18.0). On first run set Settings →
Directory → File Browser to `config/ROMs/` and System/BIOS to
`config/BIOS/`; these paths persist.

## File Transfers

tvOS has no Files app; RetroArch's built-in servers handle all transfers
and RetroArch must be running.

<details open>
<summary><b>Servers</b></summary>

| Server | Port | Endpoint |
|--------|------|----------|
| Web interface | 80 | `http://<atv-ip>` or `http://appletv.local` |
| WebDAV (v1.20.0+) | 8080 | `http://appletv.local:8080` (Finder ⌘K / Map Network Drive) |

</details>

> [!IMPORTANT]
> Both servers are unauthenticated with no auth or TLS option. Restrict
> ports 80 and 8080 on the Apple TV's IP with router firewall rules or a
> VLAN.

```
/                              ← web interface / WebDAV root
├── retroarch.cfg
└── config/
    ├── ROMs/<system>/         ← see Systems
    ├── BIOS/                  ← case-sensitive filenames
    ├── saves/  states/        ← per-core subfolders (sort_savefiles_enable / sort_savestates_enable)
    ├── shaders/shaders_slang/ ← Online Updater → Update Slang Shaders
    └── <Core Name>/           ← per-core .cfg + .opt (retroarch-configs)
```

## Systems

Tier 1 = full speed with shaders; Tier 2 = most titles at full speed.
JIT-required systems (Dreamcast, GameCube, Wii, PS2) are not supported
on the App Store build. Folder names are a convention — Manual Scan
matches against the system database you select.

<details open>
<summary><b>System table</b></summary>

| System | Tier | Folder | Core | BIOS (`config/BIOS/`) |
|--------|------|--------|------|-----------------------|
| NES | 1 | `nes/` | Mesen | — |
| SNES | 1 | `snes/` | Snes9x | — |
| GB / GBC / GBA | 1 | `gb/` `gbc/` `gba/` | mGBA | `gba_bios.bin` (optional) |
| Genesis / MD, Master System | 1 | `megadrive/` `mastersystem/` | Genesis Plus GX | — |
| Sega CD / Mega CD | 1 | `segacd/` | Genesis Plus GX | `bios_CD_U.bin`, `bios_CD_E.bin`, `bios_CD_J.bin` |
| PC Engine / TG-16, TurboGrafx-CD | 1 | `pce/` | Beetle PCE Fast | `syscard3.pce` (CD) |
| Neo Geo, Arcade (CPS1/2/3) | 1 | `neogeo/` `fbneo/` | FinalBurn Neo | `neogeo.zip` (also in `config/ROMs/neogeo/`) |
| Nintendo 64 | 2 | `n64/` | Mupen64Plus-Next | — |

</details>

Per-core overrides and core options for these cores ship in
[retroarch-configs](https://github.com/ryanmusante/retroarch-configs).

## Controllers

Pair via tvOS Settings → Remotes and Devices → Bluetooth. RetroArch on
tvOS recognizes at most three controllers
([#16685](https://github.com/libretro/RetroArch/issues/16685)); ghost
inputs from controllers 2+ can bleed into controller 1
([#18447](https://github.com/libretro/RetroArch/issues/18447)).

<details open>
<summary><b>Compatibility</b></summary>

| Controller | Status |
|------------|--------|
| PS5 DualSense / Edge, Xbox Series X/S | Recommended |
| PS4 DualShock 4, 8BitDo Pro 2, SteelSeries Nimbus+ | Works |
| Nintendo Switch Pro | Avoid — B button exits the app ([#18286](https://github.com/libretro/RetroArch/issues/18286)) |

</details>

## Configuration

The PS/Xbox home button opens tvOS Control Center, not RetroArch. Set
Settings → Input → Hotkeys → Menu Toggle (Controller Combo) → L3 + R3
and Hotkey Enable → Select before launching content; without the combo
there is no way back to the menu short of force-quitting. Game Focus
must stay off (`input_auto_game_focus = "0"`, shipped) — it blocks every
hotkey bind while content runs.

<details open>
<summary><b>Hotkeys</b></summary>

| Action | Combo |
|--------|-------|
| Quick Menu | L3 + R3 |
| Save / Load State | Select + R1 / Select + L1 |
| State Slot ± | Select + D-Pad Right / Left |
| Fast Forward / Rewind | Select + R2 / Select + L2 (rewind off globally; enable per-game) |
| Close Content | Select + Start (auto-saves a state; load it with Select + L1) |

</details>

<details open>
<summary><b>Tuning</b></summary>

| Key | Shipped | Change when |
|-----|---------|-------------|
| `video_hdr_enable` | `false` | tvOS output is HDR10 / Dolby Vision (valid through v1.22.x; master uses `video_hdr_mode`) |
| `audio_latency` | `64` | `48` for lower latency if audio stays clean |
| `fastforward_ratio` | `4.0` | Raise only with thermal headroom (passive A15) |
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
| Game Mode | TV (HDMI input) | On |
| Chroma subsampling | TV (HDMI input) | 4:4:4 or RGB Full |

</details>

## Shaders

The pipeline is on (`video_shader_enable = "true"`) with no global
preset. Per core: Quick Menu → Shaders → Load Preset → pick a preset →
Manage Presets → Save Core Preset. Avoid CRT-Royale, CRT-Geom-Deluxe,
Guest-Dr-Venom, Guest-Advanced and Mega Bezel — they exceed the A15 GPU
budget.

<details open>
<summary><b>Presets</b></summary>

| Shader | Use for |
|--------|---------|
| `crt/zfast-crt.slangp` | All systems; single-pass, integer-scale safe |
| `handheld/lcd-grid-v2.slangp` | mGBA (GB / GBC / GBA) for an LCD look |

</details>

## Versioning

`vMAJOR.MINOR`, in lockstep with `retroarch-configs` — one tag per
release across both repos. `MAJOR` on incompatible structural changes,
`MINOR` otherwise. `CHANGELOG.md` retains the last 5 releases.

## License

MIT © 2026 Ryan Musante. RetroArch is a separate project under GPL v3;
this guide and configuration are independent community resources.
