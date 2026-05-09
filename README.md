# RetroArch on Apple TV 4K

![version](https://img.shields.io/badge/version-3.27-blue)
![RetroArch](https://img.shields.io/badge/RetroArch-v1.22.x-green)
![license](https://img.shields.io/badge/license-MIT-green)

**RetroArch v1.22.x** · **tvOS 26** · **Apple TV 4K 3rd Gen (64 GB Wi-Fi)** · **April 2026**

RetroArch setup guide for Apple TV 4K (3rd generation). Covers installation, ROM/BIOS setup, controllers, performance tuning, and CRT shaders. Ships with a companion 74-key `retroarch.cfg`. Directory paths (ROMs, BIOS, saves, states) are set in-app per §4 — not via `retroarch.cfg`.

### Quick Start

1. Install RetroArch from the tvOS App Store.
2. Run Online Updater (Assets, Core Info, Databases, Slang Shaders).
3. Upload `retroarch.cfg`, ROMs, and BIOS files via the web interface or WebDAV.
4. Pair a Bluetooth controller and configure hotkeys.
5. Scan ROMs (Manual Scan) and launch a game.

[changelog](CHANGELOG.md)

## Table of Contents

1. [Prerequisites](#1-prerequisites)
2. [Installation](#2-installation)
3. [Storage Persistence](#3-storage-persistence)
4. [File Transfers](#4-file-transfers)
5. [ROM and BIOS Setup](#5-rom-and-bios-setup)
6. [Controllers](#6-controllers)
7. [Configuration](#7-configuration)
8. [Shaders](#8-shaders)
9. [Supported Systems and Per-Core Overrides](#9-supported-systems-and-per-core-overrides)
10. [Known Issues](#10-known-issues)
11. [Files in This Repository](#11-files-in-this-repository)
12. [Versioning](#12-versioning)
13. [License](#13-license)

## 1. Prerequisites

Minimum recommended version: **RetroArch v1.20.0** (required for WebDAV, automatic frame delay, and integer scaling enhancements).

| Component | Specification |
|-----------|---------------|
| Apple TV | 4K 3rd Gen (2022), 64 GB Wi-Fi (j255ap / A2737) — A15 Bionic |
| Controller | PS5 DualSense or Xbox Series X/S (Siri Remote is menu-only) |
| Network | Same Wi-Fi/LAN for Apple TV and computer (no Ethernet on 64 GB model) |
| Computer | Mac, Windows, or Linux with a web browser |
| tvOS | Latest version |
| RetroArch | Free from the tvOS App Store |
| ROMs / BIOS | Legally acquired; BIOS required for Sega CD, Neo Geo (see §5) |

## 2. Installation

1. **App Store:** Search for "RetroArch" (Daniel De Matteis) → tap Get.
2. **Launch:** The Welcome popup displays two URLs (local IP and Bonjour hostname). Record both — they are required for file transfers.
3. **Online Updater** (Main Menu → Online Updater): Update Assets, Core Info Files, Databases, Slang Shaders.
4. **Apply configuration:** Upload the companion `retroarch.cfg` to the root of the web interface (see [§4](#4-file-transfers)), then quit and relaunch RetroArch. Do **not** use "Save Current Configuration" before relaunching — it overwrites the uploaded file with the in-memory config.

## 3. Storage Persistence

> ⚠️ **Critical:** tvOS guarantees only **500 KB** of persistent storage per app. Everything else — ROMs, saves, BIOS, shaders — lives in purgeable cache that tvOS silently deletes when storage runs low. The 64 GB model purges more aggressively than the 128 GB model.

Since RetroArch v1.16.0, `retroarch.cfg` is mirrored to NSUserDefaults (the 500 KB persistent area); since v1.19.0, shader assets re-extract automatically when missing. Saves, states, and ROMs are not protected — back them up via WebDAV. Set **Settings → Directory → File Browser** to `config/ROMs/` and **System/BIOS** to `config/BIOS/` once on first run; these paths are not set by the companion config and persist via NSUserDefaults.

## 4. File Transfers

tvOS has no Files app. All transfers use RetroArch's built-in network servers; RetroArch must remain running.

### Web interface

Browse to the URL from the Welcome popup (e.g., `http://192.168.x.x` or `http://appletv.local`). Navigate into the `config` folder, create `ROMs` and `BIOS` subfolders per [§5](#5-rom-and-bios-setup), then drag-and-drop files.

### WebDAV (bulk transfers, RetroArch v1.20.0+)

Port 8080. macOS: Finder → ⌘K → `http://appletv.local:8080`. Windows: File Explorer → Map Network Drive → `http://appletv.local:8080`.

> ⚠️ **Security:** Web interface (port 80) and WebDAV (port 8080) provide **unauthenticated** read/write access to RetroArch's sandboxed filesystem with no option to add auth or TLS. Anyone on the same network can read or overwrite saves, states, and configuration. Mitigate with VLAN isolation or router firewall rules restricting access to ports 80 and 8080 on the Apple TV's IP.

### Filesystem layout (Apple TV)

```
/                              ← web interface / WebDAV root
├── retroarch.cfg              ← upload here (§2 step 4)
└── config/
    ├── ROMs/                  ← game files, organized by system (§5)
    ├── BIOS/                  ← system BIOS files (case-sensitive)
    ├── saves/                 ← in-game saves (.srm), sorted per core
    ├── states/                ← save states, sorted per core
    ├── shaders/shaders_slang/ ← Online Updater → Update Slang Shaders
    └── <Core Name>/           ← per-core overrides (.cfg + .opt)
        ├── Mesen/
        ├── Snes9x/
        └── ...                  (see retroarch-configs §7)
```

Saves and states auto-organize into per-core subfolders via `sort_savefiles_enable` / `sort_savestates_enable` and are backup-accessible via WebDAV from wherever they land.

## 5. ROM and BIOS Setup

### ROM folder reference

Place ROMs in `config/ROMs/<folder>/` using the folder names below (matched by RetroArch's database scanner).

| System | ROM Folder | Extensions | Core |
|--------|-----------|------------|------|
| NES | `nes/` | `.nes`, `.unf`, `.unif` | Mesen |
| SNES | `snes/` | `.sfc`, `.smc` | Snes9x |
| Game Boy | `gb/` | `.gb` | mGBA |
| Game Boy Color | `gbc/` | `.gbc` | mGBA |
| Game Boy Advance | `gba/` | `.gba` | mGBA |
| Genesis / Mega Drive | `megadrive/` | `.md`, `.gen`, `.bin` | Genesis Plus GX |
| Sega CD / Mega CD | `segacd/` | `.cue`, `.chd` | Genesis Plus GX |
| Master System | `mastersystem/` | `.sms` | Genesis Plus GX |
| PC Engine / TG-16 | `pce/` | `.pce`, `.cue`, `.chd` | Beetle PCE Fast |
| Neo Geo | `neogeo/` | `.zip` | FinalBurn Neo |
| Arcade (CPS1/2/3) | `fbneo/` | `.zip` | FinalBurn Neo |
| Nintendo 64 | `n64/` | `.n64`, `.z64`, `.v64` | Mupen64Plus-Next |

### BIOS files

Place in `config/BIOS/`. Filenames are case-sensitive.

| System | Required File(s) | Required? |
|--------|-----------------|-----------|
| Sega CD / Mega CD | `bios_CD_U.bin`, `bios_CD_E.bin`, `bios_CD_J.bin` | Yes |
| TurboGrafx-CD | `syscard3.pce` | Yes |
| Neo Geo | `neogeo.zip` (also in `config/ROMs/neogeo/`) | Yes |
| Game Boy Advance | `gba_bios.bin` | Optional |

### Scanning games

**Main Menu → Import Content → Manual Scan** — point at each system subfolder, pick the matching system/core, start scan. Playlists appear under Main Menu → Playlists.

## 6. Controllers

The Apple TV supports up to four simultaneous Bluetooth controllers at the OS level, though RetroArch on tvOS currently recognizes a maximum of three ([#16685](https://github.com/libretro/RetroArch/issues/16685)). The Siri Remote navigates menus only.

**Pairing:** Settings → Remotes and Devices → Bluetooth → put controller in pairing mode → select under "Other Devices."

| Controller | Status |
|-----------|--------|
| PS5 DualSense / Edge | **Recommended** — BT 5.1 (capped to ATV BT 5.0) |
| Xbox Series X/S Wireless | **Recommended** — full support |
| PS4 DualShock 4 / 8BitDo Pro 2 / SteelSeries Nimbus+ | Excellent |
| Nintendo Switch Pro | **Avoid** — B button exits app ([#18286](https://github.com/libretro/RetroArch/issues/18286)) |

> ⚠️ Ghost inputs from controllers 2+ may bleed into controller 1 in multi-controller setups ([#18447](https://github.com/libretro/RetroArch/issues/18447)).

## 7. Configuration

### Hotkeys

The PS/Xbox home button opens tvOS Control Center, not RetroArch. **Settings → Input → Hotkeys → Menu Toggle Controller Combo → L3 + R3** (click both thumbsticks). Optionally set an **Enable Hotkeys** modifier (e.g., Select).

| Action | Combo |
|--------|-------|
| Open Quick Menu | L3 + R3 |
| Save State / Load State | Select + R1 / Select + L1 |
| Fast Forward / Rewind | Select + R2 / Select + L2 (rewind disabled globally; enable per-game) |
| State Slot ± | Select + D-Pad Right / Left |
| Close Content | Select + Start |

Save state captures on Close Content; load manually with Select + L1. SRAM flushes every 5 min and is protected from state-load clobber. 10 auto-indexed slots rotate. Tier 2 cores pin `autosave_interval = "0"` to prevent purgeable-cache stall.

> ⚠️ Without Close Content configured, there is no way to exit a running game without force-quitting.

### Video settings

| Setting | Value | Notes |
|---------|-------|-------|
| `video_driver` | `metal` | Apple silicon |
| `video_scale_integer` | `true` | Pixel-perfect; per-core integer overscale on 224p / 240p / 304p systems |
| `video_smooth` | `false` | Required for shader rendering |
| `video_shader_enable` | `true` | Pipeline on; no global preset — assign per-core (§8) |
| `video_vsync` | `true` | Drift-guard pin |
| `video_refresh_rate` | `60.000000` | 60 Hz SDR seed; calibrate via Settings → Video → Output |
| `fps_show` | `true` | OSD frame-rate counter; no measurable GPU cost |

### Latency reduction

| Setting | Value | Notes |
|---------|-------|-------|
| `vrr_runloop_enable` | `false` | Apple TV is fixed-refresh; keeps DRC active |
| `run_ahead_enabled` / `_frames` | `false` / `2` | Tier 1 per-core `true`; Tier 2 explicit `false` |
| `run_ahead_secondary_instance` | `false` | FBN pins `true`; others inherit global `false` |
| `preemptive_frames_enable` | `false` | Per-core only; mutually exclusive with `run_ahead_enabled` |
| `video_frame_delay_auto` | `true` | Mupen pinned `false` ([#14201](https://github.com/libretro/RetroArch/issues/14201)) |
| `fastforward_ratio` | `4.0` | 4× cap; reduces thermal load on passive A15 |

### TV output

| Setting | Location | Value |
|---------|----------|-------|
| Resolution | tvOS Settings → Video and Audio | 4K SDR 60 Hz |
| Match Frame Rate / Dynamic Range | tvOS Settings → Video and Audio → Match Content | OFF / OFF |
| Audio Format | tvOS Settings → Video and Audio | Stereo |
| Reduce Loud Sounds | tvOS Settings → Video and Audio | OFF |
| Game Mode | TV settings (HDMI input) | ON |
| Chroma subsampling | TV settings (HDMI input) | YCbCr 4:4:4 or RGB Full |

Apple TV supports only QMS VRR (media frame-rate switching), not real-time game VRR. `vrr_runloop_enable` must stay **OFF** — enabling it disables Dynamic Rate Control, causing judder and audio desync on the fixed 59.94 Hz panel. Match Frame Rate (tvOS) targets AVPlayer, not emulation; leave OFF.

### Additional settings

`retroarch.cfg` ships hardening defaults (network command surfaces off; netplay public-announce / NAT traversal / MITM relay all `false`; on-demand thumbnail fetch off; cloud sync gate off), tvOS-inert driver subsystems set to `null` (bluetooth, wifi, midi, record, camera, location), reduced menu animation cost (XMB animation off, no shader pipeline background, no shadows, sublabels off), denser menus (favorites/history capped at 10), conservative auto-save (5-min SRAM flush, 10-slot rotation, save-on-close), and quiet logging. The full key list is in `retroarch.cfg`.

A handful of pins users may want to flip:

| Setting | Pinned | Notes |
|---------|--------|-------|
| `video_hdr_enable` | `false` | Explicit-off guard against Metal HDR10 negotiation; flip if tvOS is configured for HDR10 / Dolby Vision |
| `audio_latency` | `48` | ~3 frames @ 60 Hz; raise to `64` if crackle under sustained load |
| `audio_resampler_quality` | `3` | NORMAL (SINC); real flip from tvOS LOWER default at sub-1% A15 cost |
| `input_max_users` | `4` | tvOS RA hard-caps at 3 ([#16685](https://github.com/libretro/RetroArch/issues/16685)); 4th slot is forward-compat |
| `fastforward_ratio` | `4.0` | Sustained FF cap; thermal-friendly on passive A15 |
| `menu_pause_libretro` | `true` | Pauses emulation while Quick Menu open |
| `pause_nonactive` | `false` | Pinned `false` — prevents audio glitch when tvOS briefly marks app inactive |

## 8. Shaders

> Shader pipeline is enabled (`video_shader_enable = "true"`); no global preset is set. Assign per-core via Quick Menu → Shaders → Save Core Preset.

To assign a preset:

1. Launch a game → Quick Menu (L3 + R3) → Shaders → Video Shaders: **ON**.
2. Load Preset → `shaders_slang` → `crt` (or `handheld` for LCD-style systems) → select a preset.
3. Adjust parameters as needed.
4. Save Preset → **Save Core Preset**.

| GPU Cost | Shader | Best For |
|----------|--------|----------|
| Minimal | `crt/zfast-crt.slangp` | All 2D systems (Tier 1 + Tier 2); single-pass, integer-scale safe |
| Minimal | `handheld/lcd-grid-v2.slangp` | mGBA only (GBA / GB / GBC) when LCD aesthetic preferred |

`zfast-crt.slangp` exposes `BLURSCALEX`, `LOWLUMSCAN`, `HILUMSCAN`, `BRIGHTBOOST`, `MASK_DARK`, `MASK_FADE` via Quick Menu → Shaders → Shader Parameters.

**Avoid on Apple TV:** CRT-Royale, CRT-Geom-Deluxe, Guest-Dr-Venom, Guest-Advanced, all Mega Bezel shaders — exceed A15 GPU budget.

## 9. Supported Systems and Per-Core Overrides

Per-core override values and core options are maintained in [retroarch-configs](https://github.com/ryanmusante/retroarch-configs). After upload, move each `.cfg` / `.opt` into `config/<core_name>/` on the Apple TV — both load automatically.

Tier 1 = full speed with shaders; Tier 2 = most titles at full speed. JIT-required systems (Dreamcast, GameCube, Wii, PS2) are not supported on the App Store build.

| Tier | System | Core | Notes |
|------|--------|------|-------|
| 1 | NES | Mesen | Per-game `mesen_overclock` (None / Low / Medium / High / Very High) for Battletoads, Recca |
| 1 | SNES | Snes9x | Per-game `snes9x_overclock_superfx = "200%"` for SuperFX titles (Star Fox, Yoshi's Island, Doom, Stunt Race FX) |
| 1 | GB / GBC / GBA | mGBA | — |
| 1 | Genesis / MD / CD, SMS | Genesis Plus GX | Per-game BRAM (system + cart); Run Ahead per-core |
| 1 | PC Engine / TG-16 | Beetle PCE Fast | CD precache + 4× streaming; Run Ahead single-instance |
| 1 | Neo Geo, Arcade (CPS1/2/3) | FinalBurn Neo | Run Ahead 2 + secondary instance; rewind pinned `false` ([#16374](https://github.com/libretro/RetroArch/issues/16374)) |
| 2 | Nintendo 64 | Mupen64Plus-Next | tvOS Metal-only stack (angrylion + cxd4); P-core multithread pin; FrameDuping; 4P rumble parity. Frontend pins per [retroarch-configs §4](https://github.com/ryanmusante/retroarch-configs#4-frontend-override-keys) |

## 10. Known Issues

| # | Issue | Ref | Workaround |
|---|-------|-----|------------|
| 1 | Switch Pro B button exits app | [#18286](https://github.com/libretro/RetroArch/issues/18286) | Avoid Switch Pro Controller |
| 2 | Ghost inputs with multiple controllers | [#18447](https://github.com/libretro/RetroArch/issues/18447) | Use single controller or test carefully |
| 3 | RetroArch on tvOS caps at 3 simultaneous controllers despite OS supporting 4 | [#16685](https://github.com/libretro/RetroArch/issues/16685) | Limit multiplayer to 3 controllers; fourth pad not recognized |

## 11. Files in This Repository

| File | Description |
|------|-------------|
| `README.md` | This guide |
| [`CHANGELOG.md`](CHANGELOG.md) | Release history (kernel.org style) |
| `retroarch.cfg` | Drop-in configuration for Apple TV 4K 3rd Gen |
| `LICENSE` | MIT License |

## 12. Versioning

`vMAJOR.MINOR` (no patch) in lockstep with `retroarch-configs`; both repos share one tag and bump together every release. `MAJOR` increments on incompatible structural changes; `MINOR` increments on every release. `CHANGELOG.md` is kernel.org style (`# Version - Date` + bullets) and retains the last 5 MINOR entries.

## 13. License

[MIT](LICENSE)

RetroArch is a separate project licensed under GPL v3. This guide and configuration file are independent community resources.
