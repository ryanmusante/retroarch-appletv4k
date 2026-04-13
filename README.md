# RetroArch on Apple TV 4K

![version](https://img.shields.io/badge/version-2.75-blue)
![RetroArch](https://img.shields.io/badge/RetroArch-v1.22.x-green)
![license](https://img.shields.io/badge/license-MIT-green)

**RetroArch v1.22.x** · **tvOS 26** · **Apple TV 4K 3rd Gen (64 GB Wi-Fi · j255ap · A2737)** · **April 2026**

This package now ships a **conservative baseline** configuration. Global low-latency features that require per-core validation remain disabled in the baseline `retroarch.cfg`, but the companion Tier 1 core overrides explicitly enable Run Ahead where it has been validated.

RetroArch setup guide for Apple TV 4K (3rd generation). Covers installation, ROM/BIOS setup, controllers, performance tuning, and CRT shaders. Includes a companion `retroarch.cfg`; File Browser and System/BIOS directory paths are preset to `config/ROMs/` and `config/BIOS/`.

### Quick Start

1. Install RetroArch from the tvOS App Store.
2. Run Online Updater (Assets, Core Info, Databases, Slang Shaders).
3. Upload `retroarch.cfg`, ROMs, and BIOS files via the web interface or WebDAV.
4. Pair a Bluetooth controller and configure hotkeys.
5. Scan ROMs (Manual Scan) and launch a game.

Detailed instructions for each step follow below.

[changelog](CHANGELOG.md)

## Table of Contents

1. [Prerequisites](#1-prerequisites)
   - [Hardware](#hardware)
   - [Software](#software)
2. [Installation](#2-installation)
3. [Storage Persistence](#3-storage-persistence)
   - [Automatic config backup](#automatic-config-backup)
   - [Recommended setup](#recommended-setup)
4. [File Transfers](#4-file-transfers)
   - [Web interface](#web-interface)
   - [WebDAV (bulk transfers)](#webdav-bulk-transfers)
   - [Filesystem layout (Apple TV)](#filesystem-layout-apple-tv)
5. [ROM and BIOS Setup](#5-rom-and-bios-setup)
   - [ROM folder reference](#rom-folder-reference)
   - [BIOS files](#bios-files)
   - [Scanning games](#scanning-games)
6. [Controllers](#6-controllers)
   - [Pairing](#pairing)
   - [Compatibility](#compatibility)
7. [Configuration](#7-configuration)
   - [Hotkeys](#hotkeys)
   - [Video settings](#video-settings)
   - [Latency reduction](#latency-reduction)
   - [TV output](#tv-output)
   - [Additional settings](#additional-settings)
   - [Netplay](#netplay)
8. [CRT Shaders](#8-crt-shaders)
   - [Applying a shader](#applying-a-shader)
   - [Recommended presets](#recommended-presets)
9. [Supported Systems and Per-Core Overrides](#9-supported-systems-and-per-core-overrides)
   - [Systems not supported (JIT required)](#systems-not-supported-jit-required)
10. [Known Issues](#10-known-issues)
11. [Setup Checklist](#11-setup-checklist)
    - [Install and configure](#install-and-configure)
    - [Controllers and input](#controllers-and-input)
    - [Content](#content)
    - [tvOS and TV settings](#tvos-and-tv-settings)
    - [In-app calibration](#in-app-calibration)
    - [Network security](#network-security)
    - [Verify after relaunch](#verify-after-relaunch)
    - [Per-core overrides](#per-core-overrides)
12. [Files in This Repository](#12-files-in-this-repository)
13. [Versioning](#13-versioning)
14. [License](#14-license)

## 1. Prerequisites

Minimum recommended version: **RetroArch v1.20.0** (required for WebDAV, automatic frame delay, and integer scaling enhancements). Automatic config backup was added in v1.16.0 and asset re-extraction in v1.19.0.

### Hardware

| Component | Specification | Notes |
|-----------|---------------|-------|
| Apple TV | 4K 3rd Gen (2022), 64 GB Wi-Fi (j255ap / A2737) | A15 Bionic; 64 GB — more aggressive cache purges than 128 GB |
| Controller | PS5 DualSense or Xbox Series X/S | Any Bluetooth gamepad; Siri Remote is menu-only |
| Network | Same Wi-Fi/LAN for Apple TV and computer | Wi-Fi only (no Ethernet on 64 GB model) |
| Computer | Mac, Windows, or Linux with a web browser | Required for ROM/BIOS file transfers |

### Software

| Software | Details |
|----------|---------|
| tvOS | Latest version installed |
| RetroArch | Free from the tvOS App Store |
| Web browser | Safari, Chrome, Firefox, or Edge on the transfer computer |
| ROM files | Legally acquired game files for systems you own |
| BIOS files | Required for PS1, Sega CD, Neo Geo (see [ROM and BIOS Setup](#5-rom-and-bios-setup)) |

## 2. Installation

No sideloading, developer accounts, or jailbreaking required.

1. **App Store:** Search for "RetroArch" (Daniel De Matteis) → tap Get.
2. **Launch:** The Welcome popup displays two URLs (local IP and Bonjour hostname). Record both — they are required for file transfers.
3. **Online Updater** (Main Menu → Online Updater):
   - Update Assets (icons, fonts, overlays)
   - Update Core Info Files
   - Update Databases
   - Update Slang Shaders
4. **Apply configuration:** Upload the companion `retroarch.cfg` to the root of the web interface (see [Filesystem layout](#filesystem-layout-apple-tv)), then quit and relaunch RetroArch. Do **not** use "Save Current Configuration" before relaunching — it overwrites the uploaded file with the in-memory config.

> **Note:** RetroArch v1.20.0 added Bluetooth keyboard support on tvOS for text input, search, and core option editing.

## 3. Storage Persistence

> ⚠️ **Critical:** Understanding tvOS storage behavior is essential to preventing data loss.

tvOS guarantees only **500 KB** of persistent storage per app. All other data — ROMs, saves, BIOS files, shaders — resides in purgeable cache that tvOS silently deletes when storage runs low. The 64 GB model is more aggressive about cache purges than the 128 GB model.

### Automatic config backup

Since v1.16.0, RetroArch stores `retroarch.cfg` in NSUserDefaults (the 500 KB persistent area). Since v1.19.0, shader assets are re-extracted automatically when detected as missing.

### Recommended setup

Follow the [Filesystem layout](#filesystem-layout-apple-tv) in §4. Create the directory structure via the web interface or WebDAV:

1. Create `ROMs/` and `BIOS/` folders inside `config/`.
2. Upload ROM and BIOS files into the appropriate subfolders.
3. The companion `retroarch.cfg` sets **File Browser** to `config/ROMs/`. Verify under Settings → Directory after upload.
4. The companion `retroarch.cfg` sets **System/BIOS** to `config/BIOS/`. Verify under Settings → Directory after upload.

## 4. File Transfers

tvOS has no Files app. All transfers use RetroArch's built-in network servers. RetroArch must remain running during all transfers.

### Web interface

1. Ensure the transfer computer and Apple TV are on the same network.
2. Browse to the URL from the Welcome popup (e.g., `http://192.168.x.x` or `http://appletv.local`).
3. Navigate into the `config` folder.
4. Create `ROMs` and `BIOS` folders with subfolders per system (see [ROM and BIOS Setup](#5-rom-and-bios-setup)).
5. Drag and drop files into the appropriate subfolder.

### WebDAV (bulk transfers)

Available in RetroArch v1.20.0+. Port 8080.

| Platform | Connection |
|----------|-----------|
| macOS | Finder → Go → Connect to Server (⌘K) → `http://appletv.local:8080` |
| Windows | File Explorer → right-click This PC → Map Network Drive → `http://appletv.local:8080` |

> **Note:** The 64 GB Wi-Fi model has no Ethernet port. Use 5 GHz Wi-Fi for faster transfers and position the Apple TV near the router.

> ⚠️ **Security:** The built-in web interface and WebDAV server (port 8080) provide **unauthenticated** read/write access to RetroArch's sandboxed filesystem. No configuration settings exist to add authentication, enable TLS, or disable these services. Any device on the same network can read or overwrite saves, states, and configuration. Mitigate with VLAN isolation or router firewall rules restricting access to ports 80 and 8080 on the Apple TV's IP.

### Filesystem layout (Apple TV)

```
/                              ← web interface / WebDAV root
├── retroarch.cfg              ← upload here (§2 step 4)
└── config/
    ├── ROMs/                  ← game files, organized by system
    │   ├── nes/
    │   ├── snes/
    │   ├── psx/
    │   └── ...                   (see §5 ROM folder reference)
    ├── BIOS/                  ← system BIOS files (case-sensitive)
    ├── saves/                 ← in-game saves (.srm), sorted per core
    ├── states/                ← save states, sorted per core
    ├── Mesen/                 ← per-core overrides (from retroarch-configs repo)
    │   ├── Mesen.cfg            (.cfg = frontend overrides)
    │   └── Mesen.opt            (.opt = core options)
    ├── Snes9x/
    │   ├── Snes9x.cfg
    │   └── Snes9x.opt
    ├── ...                      (see §9 for override values)
    └── shaders_slang/
        └── crt/               ← CRT shader presets (see §8)
```

The web interface and WebDAV expose RetroArch's sandboxed root. All paths in this guide are relative to that root. The `config/` directory stores user configuration. Per-core overrides auto-load from `config/<core_name>/` and can be created via Quick Menu → Overrides → Save Core Overrides.

> **Saves and savestates:** `retroarch.cfg` sets `savefile_directory = "config/saves"` and `savestate_directory = "config/states"`, backup-accessible via WebDAV. With `sort_savefiles_enable` / `sort_savestates_enable` at their upstream defaults of `true`, both are auto-organized into per-core subfolders (e.g. `config/saves/Mesen/`, `config/states/Snes9x/`).

## 5. ROM and BIOS Setup

### ROM folder reference

Place ROMs in `config/ROMs/<folder>/` using the folder names below. These names match RetroArch's database scanner and are used for Manual Scan.

| System | ROM Folder | Extensions | Core | Tier |
|--------|-----------|------------|------|------|
| NES | `nes/` | `.nes`, `.unf`, `.unif` | Mesen | 1 |
| SNES | `snes/` | `.sfc`, `.smc` | Snes9x | 1 |
| Game Boy | `gb/` | `.gb` | mGBA | 1 |
| Game Boy Color | `gbc/` | `.gbc` | mGBA | 1 |
| Game Boy Advance | `gba/` | `.gba` | mGBA | 1 |
| Genesis / Mega Drive | `megadrive/` | `.md`, `.gen`, `.bin` | Genesis Plus GX | 1 |
| Sega CD / Mega CD | `segacd/` | `.cue`, `.chd` | Genesis Plus GX | 1 |
| Master System | `mastersystem/` | `.sms` | Genesis Plus GX | 1 |
| PC Engine / TG-16 | `pce/` | `.pce`, `.cue`, `.chd` | Beetle PCE Fast | 1 |
| Neo Geo | `neogeo/` | `.zip` | FinalBurn Neo | 1 |
| Arcade (CPS1/2/3) | `fbneo/` | `.zip` | FinalBurn Neo | 1 |
| PlayStation 1 | `psx/` | `.cue`, `.bin`, `.chd`, `.pbp` | PCSX-ReARMed | 2 |
| Nintendo 64 | `n64/` | `.n64`, `.z64`, `.v64` | Mupen64Plus-Next | 2 |

Tier definitions: **1** = Flawless (full speed, shaders enabled), **2** = Good (most titles at full speed). See [Supported Systems and Per-Core Overrides](#9-supported-systems-and-per-core-overrides) for details.

Nintendo 64 uses the companion `retroarch-configs` override pack with `Mupen64Plus-Next` set to the **Angrylion** software renderer, `cxd4` RSP, and Slang shaders.

### BIOS files

Most 8/16-bit cores include built-in BIOS support. The following 32-bit+ systems require external BIOS files placed in `config/BIOS/`.

| System | Required File(s) | Required? |
|--------|-----------------|-----------|
| PlayStation 1 | `scph5501.bin` (NA) or `scph1001.bin` | Yes |
| Sega CD / Mega CD | `bios_CD_U.bin`, `bios_CD_E.bin`, `bios_CD_J.bin` | Yes |
| TurboGrafx-CD | `syscard3.pce` | Yes |
| Neo Geo | `neogeo.zip` | Yes |
| Game Boy Advance | `gba_bios.bin` | Optional |

Neo Geo requires `neogeo.zip` in **both** `config/BIOS/` and `config/ROMs/neogeo/`. BIOS filenames are case-sensitive — use exact names (e.g., `scph5501.bin`, not `SCPH5501.BIN`).

### Scanning games

Use **Main Menu → Import Content → Manual Scan** — point at each system subfolder in `config/ROMs/`, choose the matching system/core, start scan. Playlists appear under Main Menu → Playlists.

## 6. Controllers

The Apple TV supports up to four simultaneous Bluetooth controllers. The Siri Remote navigates menus only and cannot be used as a gamepad.

### Pairing

1. **Apple TV:** Settings → Remotes and Devices → Bluetooth.
2. Put the controller into pairing mode (see table below).
3. Select the controller under "Other Devices."
4. Launch RetroArch — the controller is recognized automatically.

### Compatibility

| Controller | Pairing Method | Status |
|-----------|---------------|--------|
| PS5 DualSense / Edge | PS + Create buttons | **Recommended** — BT 5.1 (limited to ATV BT 5.0) |
| Xbox Series X/S Wireless | Xbox + Connect button | **Recommended** — full support |
| PS4 DualShock 4 | PS + Share buttons | Excellent — most stable |
| 8BitDo Pro 2 / Ultimate | Mode D/A + Pair button | Excellent — multi-platform |
| SteelSeries Nimbus+ | Bluetooth button | Good — MFi certified |
| Nintendo Switch Pro | Sync button | **Avoid** — B button exits app ([#18286](https://github.com/libretro/RetroArch/issues/18286)) |

> **Warning:** Ghost inputs from controllers 2+ may bleed into controller 1 in multi-controller setups. Test before relying on multiplayer ([#18447](https://github.com/libretro/RetroArch/issues/18447)).

## 7. Configuration

### Hotkeys

The PS/Xbox home button opens tvOS Control Center, not RetroArch's menu. A controller combo is required to access the Quick Menu.

**Menu toggle:** Settings → Input → Hotkeys → Menu Toggle Controller Combo → **L3 + R3** (click both thumbsticks). Optionally set an **Enable Hotkeys** modifier (e.g., Select/Share).

**Recommended bindings** (with Select as the Enable Hotkeys modifier):

| Action | Combo |
|--------|-------|
| Open Quick Menu | L3 + R3 |
| Save State | Select + R1 |
| Load State | Select + L1 |
| Fast Forward | Select + R2 (hold) |
| Rewind | Select + L2 (hold; rewind disabled globally; enable per-game) |
| State Slot + | Select + D-Pad Right |
| State Slot − | Select + D-Pad Left |
| Close Content | Select + Start |

> ⚠️ **Save state is not auto-captured on Close Content.** As of v2.65, `savestate_auto_save` / `savestate_auto_load` are unset (upstream default `false`). Closing content without first triggering Save State (Select + R1) discards in-memory state; SRAM is still flushed via `block_sram_overwrite` + `autosave_interval = "60"`.

> ⚠️ **Warning:** Without Close Content configured, there is no method to exit a running game without force-quitting the app.

### Video settings

| Setting | Value | Notes |
|---------|-------|-------|
| Video driver | Metal | Best performance on Apple silicon |
| Integer Scale | ON | Pixel-perfect output; produces borders at 4K |
| Integer Overscale | Optional | Per-core: NES, SNES, Genesis, PCE/TG-16, mGBA (incl. GBA 240×160), FBN (224p–304p) |
| Bilinear Filtering | OFF | Required for correct shader rendering |
| Video Shaders | ON (`video_shader_enable = "true"`) | Master shader pipeline gate; required for global `crt-easymode` auto-load (see §8) |
| Metal Argument Buffers | ON (test) | v1.22.1+; reduces CPU draw-call overhead on A15; revert if visual glitches |
| GPU Screenshot | ON | Post-shader capture; required for accurate screenshots |
| Crop Overscan | ON | Trims garbage border pixels; core-dependent |
| Max Swapchain Images | 2 | Double-buffer; lower latency than triple on fixed-refresh tvOS; revert to `3` if pacing artifacts |
| Swap Interval | 1 (`video_swap_interval = "1"`) | 60 Hz swap; set to `2` for 30 fps content on a 60 Hz display |
| Refresh Rate | Calibrate | Seeds `59.940060` (NTSC); calibrate via Settings → Video → Output → Estimated Screen Refresh Rate |

### Latency reduction

| Setting | Value | Notes |
|---------|-------|-------|
| Sync to Exact Content Framerate | OFF (`vrr_runloop_enable = "false"`) | Fixed-refresh Apple TV; keeps DRC (`audio_rate_control_delta`) active |
| Run Ahead | OFF globally (`run_ahead_enabled = "false"`, `run_ahead_frames = "1"`) | Tier 1 per-core overrides set `true` with frames = 2; Tier 2 inherits off |
| Run-Ahead Mode | Single Instance (`run_ahead_secondary_instance = "false"`) | ~½ CPU cost vs dual-instance; A15 thermal-safe; Tier 1: frames = 2; Beetle PCE Fast = 1 (CDROM); flip to `true` per-core for crackle/serialization |
| Preemptive Frames | OFF (`preemptive_frames_enable = "false"`) | RA v1.15.0 ([PR #14832](https://github.com/libretro/RetroArch/pull/14832); menu [PR #17093](https://github.com/libretro/RetroArch/pull/17093)); reruns core on input change; reuses `run_ahead_frames`; exclusive with `run_ahead_enabled` (`runahead_change_handler`); per-core only |
| Automatic Frame Delay | ON (`video_frame_delay_auto = "true"`) | Tier 1 per-core opt-in; Mupen64Plus-Next pinned `"false"` (N64 incompatible, [#14201](https://github.com/libretro/RetroArch/issues/14201)) |
| Fast Forward Ratio | 3× (`fastforward_ratio = "3.0"`) | From 5×; A15 thermal headroom; may not function with Metal on tvOS |
| Static Frame Delay | 0 | Explicit baseline; nonzero values should be tested per-core |
| Input Poll Mode | Late (`input_poll_type_behavior = "2"`) | Late input sampling; ~0.5–1 frame reduction; netplay forces `0` |

### TV output

| Setting | Location | Value |
|---------|----------|-------|
| Resolution | tvOS Settings → Video and Audio | 4K SDR 60 Hz |
| Match Frame Rate | tvOS Settings → Video and Audio → Match Content | OFF |
| Match Dynamic Range | tvOS Settings → Video and Audio → Match Content | OFF |
| Audio Format | tvOS Settings → Video and Audio | Stereo |
| Reduce Loud Sounds | tvOS Settings → Video and Audio | OFF |
| Game Mode | TV settings (HDMI input) | ON |
| Chroma subsampling | TV settings (HDMI input) | YCbCr 4:4:4 or RGB Full |

Apple TV supports only QMS VRR (media frame-rate switching), not real-time game VRR. `vrr_runloop_enable` must be **OFF** — enabling it disables Dynamic Rate Control, causing judder and audio desync on the fixed 59.94 Hz panel. Match Frame Rate (tvOS) should also be **OFF** — it targets AVPlayer video playback, not emulation.

### Additional settings

The companion `retroarch.cfg` includes hardening, input, menu performance, and logging settings. Command and remote-control interfaces are disabled by default.

| Category | Setting | Value | Notes |
|----------|---------|-------|-------|
| Security | Network/stdin Command interfaces + Camera/Location access | OFF | Zero-auth UDP (55355, 55400–55420) + stdin = input-injection vectors; camera/location blocked on top of sandbox |
| Security | On-Demand Thumbnails | OFF (`network_on_demand_thumbnails = "false"`) | Hangs on game/state load when thumbnail server is slow ([#17242](https://github.com/libretro/RetroArch/issues/17242)) |
| Cloud | Cloud Sync | OFF | `cloud_sync_enable = "false"`; driver, saves, configs, thumbs, system sub-keys all `"false"` — no backend configured; all six keys explicit for audit |
| Network | Netplay Public Announce | OFF (`netplay_public_announce = "false"`) | Upstream default ON; keeps device off public announce list |
| Network | Discord RPC | OFF (`discord_allow = "false"`) | Upstream default OFF; explicit for audit |
| Menu | Widgets (Animated Notifications) | OFF (`menu_enable_widgets = "false"`) | Upstream default ON; animated toasts add GPU compositing overhead |
| Video | Waitable Swapchains | OFF (`video_waitable_swapchains = "false"`) | Upstream ON on tvOS/Metal; pacing overhead fixed-refresh tvOS doesn't need |
| Input | Joypad Driver | mfi (`input_joypad_driver = "mfi"`) | Apple GCController framework; only viable driver on tvOS |
| Input | Max Users | 1 (`input_max_users = "1"`) | Solo play; raise to 2–4 for multiplayer |
| Menu | Favorites / History Size | 10 / 10 | Default 200; reduced for 4 GB RAM |
| Menu | Pause on Menu | ON (`menu_pause_libretro = "true"`) | Pauses emulation in Quick Menu; thermal relief for A15 |
| Menu | Pause on Focus Loss | ON (`pause_nonactive = "true"`) | Pauses on Siri/notification/app switch; reduces OS kill probability |
| Menu | Playlist Sub-labels | OFF (`playlist_show_sublabels = "false"`) | Disables per-entry metadata lookups; prevents XMB scroll lag |
| Saving | Auto-Index States | ON (`savestate_auto_index = "true"`) | Required for `savestate_max_keep = "5"` slot rotation |
| Saving | Max Auto-Increment States | 5 (`savestate_max_keep = "5"`) | Caps auto-slot growth on 64 GB cache |
| Saving | Save State Compression | ON (`savestate_file_compression = "true"`) | Reduces save state size |
| Saving | SaveRAM Compression | ON (`save_file_compression = "true"`) | Reduces SRAM size |
| Saving | State Thumbnails | ON (`savestate_thumbnail_enable = "true"`) | Slot previews; improves couch-distance readability |
| Logging | Verbosity / File Logging | OFF | `os_log` per-message alloc cost; file writes consume volatile cache |
| Audio | `audio_out_rate` | 48000 Hz | Native HDMI rate; no resampling |
| Audio | Audio Latency | 48 ms (`audio_latency = "48"`) | From 64 ms baseline; raise per-core if crackling |
| Audio | Resampler Quality | 2 (`audio_resampler_quality = "2"`) | Lower than upstream 3; acceptable quality on ATV4K |
| Audio | Audio Sync | ON (`audio_sync = "true"`) | Ties audio to `video_refresh_rate`; works with `audio_rate_control_delta` |
| Audio | Rate Control Delta | 0.008 (`audio_rate_control_delta = "0.008"`) | DRC headroom from upstream 0.005; handles tvOS clock drift |
| Video | Threaded Video | OFF (`video_threaded = "false"`) | Force-disabled by upstream for all Apple platforms ([#14978](https://github.com/libretro/RetroArch/issues/14978)); Tier 2 cfgs pin as anchor |
| Menu | Playlist Compression | ON (`playlist_compression = "true"`) | ~90% reduction; reduces cache writes on volatile tvOS storage |
| System | Screensaver Suspend | ON (`suspend_screensaver_enable = "true"`) | Requests screensaver off during content; tvOS may still fire while paused |
| Latency | Run Ahead Hide Warnings | ON (`run_ahead_hide_warnings = "true"`) | Per-core overrides handle incompatible cores; suppresses noise |

See also the [WebDAV security warning](#4-file-transfers) in §4.

### Netplay

RetroArch supports peer-to-peer Netplay with up to 16 players and spectators. A low-latency network is recommended (5 GHz Wi-Fi or Ethernet on the 128 GB model). Only cores with serialization (save state) support are compatible.

## 8. CRT Shaders

### Applying a shader

As of v2.55, `retroarch.cfg` sets a **global default CRT shader** of `shaders_slang/crt/crt-easymode.slangp`, which every core inherits automatically. The companion `retroarch-configs` v1.27+ Tier 2 overrides (`Mupen64Plus-Next.cfg`, `PCSX-ReARMed.cfg`) replace it with the lighter `zfast_crt.slangp` to fit the interpreter + software-RDP GPU budget. No manual steps are required for the defaults to load. To override the preset for a specific core:

1. Launch a game → Quick Menu (L3 + R3) → Shaders → Video Shaders: **ON**.
2. Load Preset → `shaders_slang` → `crt` → select a preset.
3. Adjust parameters as needed (see **crt-easymode 4K parameters** below).
4. Save Preset → **Save Core Preset** (overrides the `.cfg` assignment for that core).

### Recommended presets

Grouped by GPU cost at 4K output on the passively-cooled A15:

| GPU Cost | Shader | Best For |
|----------|--------|----------|
| Minimal | `zfast_crt.slangp` | All systems, especially PS1/N64 |
| Minimal | `crt-pi.slangp` | Heaviest cores; comparable weight to zfast_crt |
| Minimal | `crt-potato-warm/cool.slangp` | All systems (lookup-texture based) |
| Low | `crt-easymode.slangp` | NES, SNES, Genesis, PC Engine/TurboGrafx-16, GBA |
| Low | `crt-hyllian.slangp` | SNES, Genesis (Trinitron aesthetic) |
| Low | `crt-aperture.slangp` | SNES, Genesis (lighter than crt-hyllian) |
| Low | `fakelottes.slangp` | 16-bit systems (lighter crt-lottes) |
| Medium | `crt-geom.slangp` | All 2D systems (scanlines + curvature) |
| Medium | `crt-lottes-fast.slangp` | 16-bit systems (slot mask + bloom) |

Use Minimal presets for Tier 2 cores where GPU headroom is limited by interpreter overhead. If complex shaders cause frame drops or thermal throttling at 4K, switch Apple TV output to 1080p SDR 60 Hz — the TV's scaler handles upscaling, and GPU load drops significantly.

**Avoid on Apple TV:** CRT-Royale, CRT-Geom-Deluxe, Guest-Dr-Venom, Guest-Advanced, and all Mega Bezel shaders exceed the A15's GPU budget.

> **Integer Scaling Conflict:** Multi-pass CRT shaders that perform their own geometry (crt-geom, crt-hyllian) expect to control the full output resolution. `video_scale_integer = ON` constrains the viewport *before* the shader processes it, resulting in a smaller and potentially distorted image. Set `video_scale_integer = OFF` in per-core overrides where geometry shaders are used. Simple scanline-only shaders (zfast_crt, crt-pi) are unaffected.

**crt-easymode 4K parameters** (community-recommended starting point): Mask Strength 0.18, Mask Type 1, Scanline Strength 0.95, Gamma Input 2.2, Gamma Output 1.8, Brightness 1.10. Adjust Mask Strength to taste for your display.

> **Note:** If `shaders_slang/crt/` appears empty after an update, re-run Online Updater → Update Slang Shaders. If that fails, upload presets manually via WebDAV to `config/shaders_slang/crt/`.

## 9. Supported Systems and Per-Core Overrides

The A15 Bionic handles retro emulation effectively, but Apple's App Store restriction on JIT compilation limits performance for demanding systems. Dreamcast, GameCube, Wii, and PS2 require JIT and cannot run through the App Store version.

Per-core override values and core options are maintained in the companion [retroarch-configs](https://github.com/ryanmusante/retroarch-configs) repository. That ZIP ships its files flat under `config/`; after upload, move each `.cfg` / `.opt` file into the matching `config/<core_name>/` directory on the Apple TV (see [Filesystem layout](#filesystem-layout-apple-tv)). Once placed there, both file types load automatically — no manual entry required inside RetroArch. Tier 1 `.cfg` files explicitly enable Run Ahead per core; the global baseline in this repository does not enable it for all cores.

Tier definitions: **1** = Flawless (full speed, shaders enabled), **2** = Good (most titles at full speed).

| Tier | System | Core | Override | Notes |
|------|--------|------|----------|-------|
| 1 | NES | Mesen | Yes | Overscale 224p; per-game: `mesen_overclock_rate` for slowdown-prone titles (Battletoads, Recca) |
| 1 | SNES | Snes9x | Yes | Overscale 224p; per-game: `snes9x_overclock = 200` for SuperFX (Star Fox, Yoshi's Island, Doom, Stunt Race FX) |
| 1 | GB / GBC / GBA | mGBA | Yes | — |
| 1 | Genesis / MD / CD, Master System | Genesis Plus GX | Yes | Overscale 224p; Run Ahead per-core; Master System (192p) may need per-content override |
| 1 | PC Engine / TG-16 | Beetle PCE Fast | Yes | — |
| 1 | Neo Geo, Arcade (CPS1/2/3) | FinalBurn Neo | Yes | Rewind pinned `false` per-core; do not re-enable per-game alongside Run Ahead ([#16374](https://github.com/libretro/RetroArch/issues/16374)) |
| 2 | PlayStation 1 | PCSX-ReARMed | Yes | No JIT; Run Ahead + preemptive inherit off; re-enable per-game for light 2D; `psxclock = 100` native; underclock 75/50 for 3D (Tony Hawk, Spyro 2/3, Tekken 3) |
| 2 | Nintendo 64 | Mupen64Plus-Next | Yes | ~60–70% compat; Angrylion sw RDP + CXD4; pins: `rewind_enable=false` ([#18300](https://github.com/libretro/RetroArch/issues/18300)), `video_frame_delay_auto=false` ([#14201](https://github.com/libretro/RetroArch/issues/14201)); Run Ahead off; re-enable per-game |

### Systems not supported (JIT required)

Dreamcast, GameCube, Wii, and PS2 require JIT compilation. The App Store version of RetroArch cannot execute JIT-compiled code. This is an Apple policy constraint, not a hardware limitation.

## 10. Known Issues

| # | Issue | Ref | Status | Workaround |
|---|-------|-----|--------|------------|
| 1 | Switch Pro B button exits app | [#18286](https://github.com/libretro/RetroArch/issues/18286) | Open | Avoid Switch Pro Controller |
| 2 | Ghost inputs with multiple controllers | [#18447](https://github.com/libretro/RetroArch/issues/18447) | Open | Use single controller or test carefully |
| 3 | Mupen64Plus-Next rewind feature request | [#18300](https://github.com/libretro/RetroArch/issues/18300) | Open | `rewind_enable = "false"` per-core + global; do not re-enable per-game |
| 4 | Mupen64Plus-Next auto frame delay incompatible | [#14201](https://github.com/libretro/RetroArch/issues/14201) | Open | `video_frame_delay_auto = "false"` per-core (global `true` as of v2.66); do not re-enable per-game; refactored upstream v1.20.0 |
| 5 | N64 rendering glitches (game-specific) | [#16598](https://github.com/libretro/RetroArch/issues/16598) | Open | Per-game overrides |
| 6 | Threaded video force-disabled on all Apple platforms | [#14978](https://github.com/libretro/RetroArch/issues/14978) | Persists | Upstream `gfx/video_driver.c` `#if defined(__MACH__) && defined(__APPLE__)` — NSWindow/UIWindow concurrency; no upstream fix; `video_threaded=false` globally; Tier 2 cfgs pin |

## 11. Setup Checklist

> **Note:** Uploading `retroarch.cfg` (§2 step 4) applies all video, audio, latency, security, menu, and logging settings. This checklist covers only actions and settings that the config file cannot control.

### Install and configure

- [ ] RetroArch installed from tvOS App Store
- [ ] Online Updater completed: Assets, Core Info, Databases, Slang Shaders
- [ ] `retroarch.cfg` uploaded via web/WebDAV and loaded on relaunch (do **not** Save Current Config first)

### Controllers and input

- [ ] Bluetooth controller paired (DualSense or Xbox Series recommended)
- [ ] Menu Toggle verified (L3 + R3 opens Quick Menu in-game)
- [ ] Hotkeys configured (Settings → Input → Hotkeys)
- [ ] Close Content hotkey set (Select + Start)

### Content

- [ ] ROMs placed in `config/ROMs/<s>/` (see [ROM folder reference](#rom-folder-reference))
- [ ] BIOS files placed in `config/BIOS/` (case-sensitive filenames)
- [ ] Manual Scan completed per system

### tvOS and TV settings

- [ ] All seven settings in [TV output (§7)](#tv-output) applied

### In-app calibration

- [ ] Aspect Ratio verified as Core Provided (preset by `aspect_ratio_index = "22"`)
- [ ] `video_refresh_rate` calibrated to your display (Settings → Video → Output → Estimated Screen Refresh Rate)

### Network security

- [ ] Web interface and WebDAV (ports 80 and 8080) access restricted at network level (VLAN/firewall)

### Verify after relaunch

- [ ] CRT shader loads automatically (global `crt-easymode` for Tier 1; Tier 2 overrides to `zfast_crt` per `retroarch-configs` v1.27+). Verify via Quick Menu → Shaders

### Per-core overrides

- [ ] Tier 1–2 override files (`.cfg` and `.opt`) uploaded to `config/<core_name>/` (see §9)
- [ ] Both file types load automatically — verify via Quick Menu → Information

## 12. Files in This Repository

| File | Description |
|------|-------------|
| `README.md` | This guide |
| [`CHANGELOG.md`](CHANGELOG.md) | Release history (kernel.org style) |
| `retroarch.cfg` | Drop-in configuration for Apple TV 4K 3rd Gen |
| `LICENSE` | MIT License |

## 13. Versioning

This repository uses `vMAJOR.MINOR` (no patch component). `MAJOR` increments on incompatible structural changes (filesystem layout, breaking config schema, removed features). `MINOR` increments on every release — additive changes, key additions/removals, documentation syncs, and conservative defaults adjustments. Each release ships with a matching `CHANGELOG.md` entry in kernel.org `date<TAB>name` style.

## 14. License

[MIT](LICENSE)

RetroArch is a separate project licensed under GPL v3. This guide and configuration file are independent community resources.
