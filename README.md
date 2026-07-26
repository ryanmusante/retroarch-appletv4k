# retroarch-appletv4k

[![version](https://img.shields.io/badge/version-4.2-blue.svg)](CHANGELOG.md)
[![RetroArch](https://img.shields.io/badge/RetroArch-v1.22.x-green.svg)](https://www.retroarch.com/)
[![tvOS](https://img.shields.io/badge/tvOS-26-black.svg)](https://www.apple.com/tvos/)
[![paired](https://img.shields.io/badge/paired-retroarch--configs%20v4.2-orange.svg)](https://github.com/ryanmusante/retroarch-configs)
![license](https://img.shields.io/badge/license-MIT-green.svg)

> RetroArch setup for Apple TV 4K (3rd Gen). Covers installation,
> ROM/BIOS, controllers, performance tuning, and CRT shaders. Ships a
> 74-key `retroarch.cfg`.

**Target:** Apple TV 4K 3rd Gen (64 GB Wi-Fi, A15 Bionic) on tvOS 26. See [Prerequisites](#prerequisites).

---

## Contents

- [Quick Start](#quick-start)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Storage Persistence](#storage-persistence)
- [File Transfers](#file-transfers)
- [ROM and BIOS Setup](#rom-and-bios-setup)
- [Controllers](#controllers)
- [Configuration](#configuration)
- [Shaders](#shaders)
- [Supported Systems](#supported-systems)
- [Known Issues](#known-issues)
- [Files in This Repository](#files-in-this-repository)
- [Versioning](#versioning)
- [License](#license)

## Quick Start

1. Install RetroArch from the tvOS App Store.
2. Run Online Updater (Assets, Core Info, Databases, Slang Shaders).
3. Upload `retroarch.cfg`, ROMs, and BIOS via the web interface or WebDAV.
4. Pair a Bluetooth controller and configure hotkeys.
5. Scan ROMs (Manual Scan) and launch a game.

> [!IMPORTANT]
> Do **not** use "Save Current Configuration" before relaunching
> after uploading `retroarch.cfg` — it overwrites the uploaded file
> with the in-memory config.

See [CHANGELOG](CHANGELOG.md) for release history.

## Prerequisites

| Component | Specification |
|-----------|---------------|
| Apple TV | 4K 3rd Gen (2022), 64 GB Wi-Fi (j255ap / A2737) — A15 Bionic |
| Controller | PS5 DualSense or Xbox Series X/S (Siri Remote is menu-only) |
| Network | Same Wi-Fi/LAN for Apple TV and computer (no Ethernet on 64 GB model) |
| Computer | Mac, Windows, or Linux with a web browser |
| tvOS | Latest version |
| RetroArch | ≥ v1.20.0 (WebDAV file transfer); v1.22.x recommended |
| ROMs / BIOS | Legally acquired; BIOS required for Sega CD, Neo Geo |

## Installation

1. **App Store:** Search "RetroArch" (Daniel De Matteis) → Get.
2. **Launch:** Record the two URLs (local IP + Bonjour) from the Welcome popup — required for file transfers.
3. **Online Updater:** Main Menu → Online Updater → Assets, Core Info Files, Databases, Slang Shaders.
4. **Apply config:** Upload the companion `retroarch.cfg` to the root of the web interface ([File Transfers](#file-transfers)), then quit and relaunch RetroArch.

## Storage Persistence

> [!IMPORTANT]
> tvOS guarantees only **500 KB** of persistent storage per app.
> Everything else — ROMs, saves, BIOS, shaders — lives in purgeable
> cache that tvOS silently deletes when storage runs low. The 64 GB
> model purges more aggressively than the 128 GB model.

Since RetroArch v1.16.0, `retroarch.cfg` is mirrored to NSUserDefaults
(the 500 KB persistent area); since v1.18.0, assets re-extract
automatically when the cache is purged. Saves, states, and ROMs are
not protected — back them up via WebDAV. Set **Settings → Directory →
File Browser** to `config/ROMs/` and **System/BIOS** to `config/BIOS/`
once on first run; these paths persist via NSUserDefaults.

## File Transfers

tvOS has no Files app. All transfers use RetroArch's built-in network
servers; RetroArch must remain running.

| Server | Port | Endpoint |
|--------|------|----------|
| Web interface | 80 | `http://<atv-ip>` or `http://appletv.local` |
| WebDAV (v1.20.0+) | 8080 | macOS Finder ⌘K · Windows Map Network Drive · `http://appletv.local:8080` |

> [!IMPORTANT]
> Web interface and WebDAV are **unauthenticated** with no option to
> add auth or TLS. Anyone on the same network can read or overwrite
> saves, states, and configuration. Mitigate with VLAN isolation or
> router firewall rules restricting access to ports 80 and 8080 on
> the Apple TV's IP.

<details>
<summary><b>Filesystem layout</b></summary>

```
/                              ← web interface / WebDAV root
├── retroarch.cfg              ← upload here (Installation step 4)
└── config/
    ├── ROMs/                  ← game files, organized by system
    ├── BIOS/                  ← system BIOS files (case-sensitive)
    ├── saves/                 ← in-game saves (.srm), sorted per core
    ├── states/                ← save states, sorted per core
    ├── shaders/shaders_slang/ ← Online Updater → Update Slang Shaders
    └── <Core Name>/           ← per-core overrides (.cfg + .opt; see retroarch-configs)
```

Saves and states auto-organize into per-core subfolders via
`sort_savefiles_enable` / `sort_savestates_enable`.

</details>

## ROM and BIOS Setup

Place ROMs in `config/ROMs/<folder>/`, one folder per system. The
names below are a convention, not a matching rule — Manual Scan
identifies content against the system database you select. Place BIOS
in `config/BIOS/`; filenames are case-sensitive.

Scan via **Main Menu → Import Content → Manual Scan**, point at each
system subfolder, pick the system/core, start scan. Playlists appear
under Main Menu → Playlists.

<details>
<summary><b>ROM folder reference</b></summary>

| System | Folder | Extensions | Core |
|--------|--------|------------|------|
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

</details>

<details>
<summary><b>BIOS files</b></summary>

| System | Required File(s) | Required? |
|--------|------------------|-----------|
| Sega CD / Mega CD | `bios_CD_U.bin`, `bios_CD_E.bin`, `bios_CD_J.bin` | Yes |
| TurboGrafx-CD | `syscard3.pce` | Yes |
| Neo Geo | `neogeo.zip` (also in `config/ROMs/neogeo/`) | Yes |
| Game Boy Advance | `gba_bios.bin` | Optional |

</details>

## Controllers

The Apple TV supports up to four simultaneous Bluetooth controllers
at the OS level, though RetroArch on tvOS currently recognizes a
maximum of three ([#16685](https://github.com/libretro/RetroArch/issues/16685)).
The Siri Remote navigates menus only.

**Pairing:** Settings → Remotes and Devices → Bluetooth → put
controller in pairing mode → select under "Other Devices."

| Controller | Status |
|------------|--------|
| PS5 DualSense / Edge | **Recommended** — BT 5.1 (capped to ATV BT 5.0) |
| Xbox Series X/S Wireless | **Recommended** — full support |
| PS4 DualShock 4 / 8BitDo Pro 2 / SteelSeries Nimbus+ | Excellent |
| Nintendo Switch Pro | **Avoid** — B button exits app ([#18286](https://github.com/libretro/RetroArch/issues/18286)) |

> [!NOTE]
> Ghost inputs from controllers 2+ may bleed into controller 1 in
> multi-controller setups ([#18447](https://github.com/libretro/RetroArch/issues/18447)).

## Configuration

The PS/Xbox home button opens tvOS Control Center, not RetroArch.
**Settings → Input → Hotkeys → Menu Toggle Controller Combo → L3 + R3**
(click both thumbsticks). Set an Enable Hotkeys modifier (Select) —
the combos below assume it.

> [!IMPORTANT]
> Configure the Menu Toggle combo before launching content. Without
> it there is no way back to the menu — and so no way to reach Close
> Content, save states, or settings — short of force-quitting.

<details>
<summary><b>Hotkeys</b></summary>

| Action | Combo |
|--------|-------|
| Open Quick Menu | L3 + R3 |
| Save State / Load State | Select + R1 / Select + L1 |
| Fast Forward / Rewind | Select + R2 / Select + L2 (rewind disabled globally; enable per-game) |
| State Slot ± | Select + D-Pad Right / Left |
| Close Content | Select + Start |

Save state captures on Close Content; load manually with Select + L1.
SRAM flushes every 5 min and is protected from state-load clobber. 10
auto-indexed slots rotate. Tier 2 cores pin `autosave_interval = "0"`
to prevent purgeable-cache stall. Game Focus must stay off
(`input_auto_game_focus = "0"`, shipped) — it blocks every hotkey
bind while content runs.

</details>

<details>
<summary><b>Video settings</b></summary>

| Setting | Value | Notes |
|---------|-------|-------|
| `video_driver` | `metal` | Apple silicon |
| `video_scale_integer` | `true` | Pixel-perfect; per-core integer overscale on 224p / 240p / 304p systems |
| `video_smooth` | `false` | Nearest-neighbour; avoids pre-shader blur |
| `video_shader_enable` | `true` | Pipeline on; no global preset — assign per-core |
| `video_vsync` | `true` | Drift-guard pin |
| `video_refresh_rate` | `60.000000` | 60 Hz SDR seed; calibrate via Settings → Video → Output |
| `fps_show` | `true` | OSD frame-rate counter; no measurable GPU cost |

</details>

<details>
<summary><b>Latency reduction</b></summary>

| Setting | Value | Notes |
|---------|-------|-------|
| `vrr_runloop_enable` | `false` | Apple TV is fixed-refresh; keeps DRC active |
| `run_ahead_enabled` / `_frames` | `false` / `2` | Tier 1 per-core `true`; Tier 2 explicit `false` |
| `run_ahead_secondary_instance` | `false` | All cores inherit; single-instance per companion repo |
| `preemptive_frames_enable` | `false` | Per-core only; mutually exclusive with `run_ahead_enabled` |
| `video_frame_delay_auto` | `true` | Mupen pinned `false` — drift-guard ([#14201](https://github.com/libretro/RetroArch/issues/14201)) |
| `fastforward_ratio` | `4.0` | 4× cap; reduces thermal load on passive A15 |

</details>

<details>
<summary><b>TV output</b></summary>

| Setting | Location | Value |
|---------|----------|-------|
| Resolution | tvOS Settings → Video and Audio | 4K SDR 60 Hz |
| Match Frame Rate / Dynamic Range | tvOS Settings → Video and Audio → Match Content | OFF / OFF |
| Audio Format | tvOS Settings → Video and Audio | Stereo |
| Reduce Loud Sounds | tvOS Settings → Video and Audio | OFF |
| Game Mode | TV settings (HDMI input) | ON |
| Chroma subsampling | TV settings (HDMI input) | YCbCr 4:4:4 or RGB Full |

> [!IMPORTANT]
> Apple TV supports only QMS VRR (media frame-rate switching), not
> real-time game VRR. `vrr_runloop_enable` must stay **OFF** —
> enabling it disables Dynamic Rate Control, causing judder and audio
> desync on the fixed 60 Hz output. Match Frame Rate (tvOS) targets
> AVPlayer, not emulation; leave OFF.

</details>

<details>
<summary><b>Additional settings</b></summary>

`retroarch.cfg` ships hardening defaults
(network/netplay/cloud-sync surfaces off; tvOS-inert driver
subsystems set to `null`), reduced menu animation cost, denser menus
(favorites/history capped at 10), conservative auto-save (5-min SRAM
flush, 10-slot rotation, save-on-close), and quiet logging. The full
key list is in `retroarch.cfg`.

| Setting | Pinned | Notes |
|---------|--------|-------|
| `video_hdr_enable` | `false` | Explicit-off guard against Metal HDR10 negotiation; flip if tvOS is configured for HDR10 / Dolby Vision |
| `audio_latency` | `64` | ~4 frames @ 60 Hz; clears libretro 3-frame safe floor, stutter-resistant under passive-A15 throttling. Lower to `48` (~3 frames) to favor latency if audio stays clean |
| `audio_resampler_quality` | `3` | NORMAL (SINC); real flip from tvOS LOWER default at sub-1% A15 cost |
| `input_max_users` | `4` | tvOS RA hard-caps at 3 ([#16685](https://github.com/libretro/RetroArch/issues/16685)); 4th slot is forward-compat |
| `fastforward_ratio` | `4.0` | Sustained FF cap; thermal-friendly on passive A15 |
| `menu_pause_libretro` | `true` | Pauses emulation while Quick Menu open |
| `pause_nonactive` | `false` | Pinned `false` — prevents audio glitch when tvOS briefly marks app inactive |
| `input_auto_game_focus` | `0` | Game Focus blocks every hotkey bind while content runs; enabling it trades all pad hotkeys for keyboard capture |

</details>

## Shaders

Shader pipeline is enabled (`video_shader_enable = "true"`); no global
preset is set. Assign per-core via Quick Menu → Shaders → Save Core
Preset.

| GPU Cost | Shader | Best For |
|----------|--------|----------|
| Minimal | `crt/zfast-crt.slangp` | All 2D systems (Tier 1 + Tier 2); single-pass, integer-scale safe |
| Minimal | `handheld/lcd-grid-v2.slangp` | mGBA only (GBA / GB / GBC) when LCD aesthetic preferred |

`zfast-crt.slangp` exposes `BLURSCALEX`, `LOWLUMSCAN`, `HILUMSCAN`,
`BRIGHTBOOST`, `MASK_DARK`, `MASK_FADE` via Quick Menu → Shaders →
Shader Parameters.

**Avoid on Apple TV:** CRT-Royale, CRT-Geom-Deluxe, Guest-Dr-Venom,
Guest-Advanced, all Mega Bezel shaders — exceed A15 GPU budget.

<details>
<summary><b>Assign procedure</b></summary>

1. Launch a game → Quick Menu (L3 + R3) → Shaders → Video Shaders: **ON**.
2. Load Preset → `shaders_slang` → `crt` (or `handheld` for LCD-style systems) → select a preset.
3. Adjust parameters as needed.
4. Save Preset → **Save Core Preset**.

</details>

## Supported Systems

Per-core override values and core options are maintained in
[retroarch-configs](https://github.com/ryanmusante/retroarch-configs).
After upload, move each `.cfg` / `.opt` into `config/<core_name>/` on
the Apple TV — both load automatically.

Tier 1 = full speed with shaders; Tier 2 = most titles at full speed.
JIT-required systems (Dreamcast, GameCube, Wii, PS2) are not
supported on the App Store build.

| Tier | System | Core | Notes |
|------|--------|------|-------|
| 1 | NES | Mesen | Per-game `mesen_overclock` for Battletoads, Recca |
| 1 | SNES | Snes9x | Per-game `snes9x_overclock_superfx = "200%"` for SuperFX (Star Fox, Yoshi's Island, Doom, Stunt Race FX) |
| 1 | GB / GBC / GBA | mGBA | — |
| 1 | Genesis / MD / CD, SMS | Genesis Plus GX | Per-game BRAM (system + cart); Run Ahead |
| 1 | PC Engine / TG-16 | Beetle PCE Fast | 2× CD read speed (full-image precache is per-game opt-in); Run Ahead single-instance |
| 1 | Neo Geo, Arcade (CPS1/2/3) | FinalBurn Neo | Run Ahead 2 single-instance; rewind `false` ([#16374](https://github.com/libretro/RetroArch/issues/16374)) |
| 2 | Nintendo 64 | Mupen64Plus-Next | tvOS Metal-only stack; P-core multithread pin; FrameDuping; 4P rumble. Frontend pins per [retroarch-configs](https://github.com/ryanmusante/retroarch-configs#configuration) |

## Known Issues

| # | Issue | Ref | Workaround |
|---|-------|-----|------------|
| 1 | Switch Pro B button exits app | [#18286](https://github.com/libretro/RetroArch/issues/18286) | Avoid Switch Pro Controller |
| 2 | Ghost inputs with multiple controllers | [#18447](https://github.com/libretro/RetroArch/issues/18447) | Use single controller or test carefully |
| 3 | tvOS caps at 3 simultaneous controllers despite OS supporting 4 | [#16685](https://github.com/libretro/RetroArch/issues/16685) | Limit multiplayer to 3 controllers |

## Files in This Repository

| File | Description |
|------|-------------|
| `README.md` | This guide |
| [`CHANGELOG.md`](CHANGELOG.md) | Release history (kernel.org style) |
| `retroarch.cfg` | Drop-in configuration for Apple TV 4K 3rd Gen |
| `LICENSE` | MIT License |

## Versioning

`vMAJOR.MINOR` (no patch), in lockstep with `retroarch-configs` —
both repos share one tag per release. `MAJOR` on incompatible
structural changes; `MINOR` on every release. `CHANGELOG.md` is
kernel.org style and retains the last 5 releases.

## License

MIT © 2026 Ryan Musante

RetroArch is a separate project licensed under GPL v3. This guide and
configuration file are independent community resources.
