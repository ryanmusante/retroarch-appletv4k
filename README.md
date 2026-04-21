# RetroArch on Apple TV 4K

![version](https://img.shields.io/badge/version-3.6-blue)
![RetroArch](https://img.shields.io/badge/RetroArch-v1.22.x-green)
![license](https://img.shields.io/badge/license-MIT-green)

**RetroArch v1.22.x** · **tvOS 26** · **Apple TV 4K 3rd Gen (64 GB Wi-Fi · j255ap · A2737)** · **April 2026**

This package ships a baseline configuration as of v3.6. The 90-key global baseline applies Metal-path latency tuning (`video_frame_rest`, `audio_latency = "48"`, `menu_pause_libretro = "false"`, `input_auto_game_focus = "1"`), explicit command-surface and HDR hardening, and XMB animation trims for the A15. The global shader pipeline is enabled (`video_shader_enable = "true"`); no preset is set in `retroarch.cfg` — users select presets per-core via Quick Menu → Shaders → Save Core Preset (see §8). Tier 1 core overrides explicitly enable Run Ahead where it has been validated. Global low-latency features that require per-core validation remain disabled in the baseline `retroarch.cfg`.

RetroArch setup guide for Apple TV 4K (3rd generation). Covers installation, ROM/BIOS setup, controllers, performance tuning, and CRT shaders. Includes a companion `retroarch.cfg`. Directory paths (ROMs, BIOS, saves, states) are set in-app per §4 — not via `retroarch.cfg`.

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
8. [Shaders](#8-shaders)
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
3. In RetroArch, set **Settings → Directory → File Browser** to `config/ROMs/` and **Settings → Directory → System/BIOS** to `config/BIOS/`. These paths are not set by the companion `retroarch.cfg`; they must be configured in-app once, then persist via NSUserDefaults.

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
    ├── shaders/               ← Online Updater → Update Slang Shaders
    │   └── shaders_slang/
    │       ├── crt/           ← CRT shader presets (see §8)
    │       └── handheld/      ← handheld LCD presets (mGBA)
    ├── Mesen/                 ← per-core overrides (from retroarch-configs repo)
    │   ├── Mesen.cfg            (.cfg = frontend overrides)
    │   └── Mesen.opt            (.opt = core options)
    ├── Snes9x/
    │   ├── Snes9x.cfg
    │   └── Snes9x.opt
    └── ...                      (see §9 for override values)
```

The web interface and WebDAV expose RetroArch's sandboxed root. All paths in this guide are relative to that root. The `config/` directory stores user configuration. Per-core overrides auto-load from `config/<core_name>/` and can be created via Quick Menu → Overrides → Save Core Overrides.

> **Saves and savestates:** `retroarch.cfg` pins `sort_savefiles_enable = "true"` and `sort_savestates_enable = "true"` (drift-guard on upstream defaults) so saves and states auto-organize into per-core subfolders (e.g. `Mesen/`, `Snes9x/`) under whichever save/state directories are configured in Settings → Directory. Both are backup-accessible via WebDAV from wherever they land.

## 5. ROM and BIOS Setup

### ROM folder reference

Place ROMs in `config/ROMs/<folder>/` using the folder names below. These names match RetroArch's database scanner and are used for Manual Scan.

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
| PlayStation 1 | `psx/` | `.cue`, `.bin`, `.chd`, `.pbp` | PCSX-ReARMed |
| Nintendo 64 | `n64/` | `.n64`, `.z64`, `.v64` | Mupen64Plus-Next |

See [Supported Systems and Per-Core Overrides](#9-supported-systems-and-per-core-overrides) for tier classification and per-core notes.

Nintendo 64 uses the companion `retroarch-configs` override pack with `Mupen64Plus-Next` set to the **Angrylion** software renderer, `cxd4` RSP, and Slang shaders.

### BIOS files

Most 8/16-bit cores include built-in BIOS support. The following CD-based, arcade, and 32-bit systems require external BIOS files placed in `config/BIOS/`.

| System | Required File(s) | Required? |
|--------|-----------------|-----------|
| PlayStation 1 | `scph5501.bin` (NA), `scph5502.bin` (EU), `scph5500.bin` (JP), or `scph1001.bin` | Yes |
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
| 8BitDo Pro 2 / Ultimate | macOS mode (M) + Pair button | Excellent — multi-platform; older firmware labels M position as A |
| SteelSeries Nimbus+ | Bluetooth button | Good — MFi certified |
| Nintendo Switch Pro | Sync button | **Avoid** — B button exits app ([#18286](https://github.com/libretro/RetroArch/issues/18286)) |

> ⚠️ **Warning:** Ghost inputs from controllers 2+ may bleed into controller 1 in multi-controller setups. Test before relying on multiplayer ([#18447](https://github.com/libretro/RetroArch/issues/18447)).

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

> ℹ️ **Save state behavior.** `savestate_auto_save = "true"` captures in-memory state on Close Content; `savestate_auto_load = "false"` keeps launches clean. Use Select + L1 to manually load the auto-saved state. SRAM is flushed every 2.5 min via `autosave_interval = "150"`; `block_sram_overwrite = "true"` prevents a manual state-load from clobbering live SRAM. Up to 10 auto-indexed slots rotate (`savestate_max_keep = "10"`). **v3.3:** Tier 2 cores pin `autosave_interval = "0"` per-core to prevent purgeable-cache stall; save-on-close still applies.

> ⚠️ **Warning:** Without Close Content configured, there is no method to exit a running game without force-quitting the app.

### Video settings

| Setting | Value | Notes |
|---------|-------|-------|
| Video driver | `video_driver = "metal"` | Apple silicon |
| Integer Scale | `video_scale_integer = "true"` | Pixel-perfect; borders at 4K. Mupen inherits but uses no overscale mode |
| Integer Overscale | Per-core | NES, SNES, Genesis, PCE, mGBA, FBN (224p–304p), PCSX (variable 256–640) |
| Bilinear Filtering | `video_smooth = "false"` | Required for shader rendering |
| Video Shaders | `video_shader_enable = "true"` | Pipeline on; no global preset — assign per-core (§8) |
| GPU Screenshot | `video_gpu_screenshot = "true"` | Post-shader capture |
| Crop Overscan | `video_crop_overscan = "true"` | Core-dependent |
| Max Swapchain Images | `video_max_swapchain_images = "2"` | Global double-buffer; v3.3 Tier 2 per-core pin = `3` (Mupen + PCSX) |
| Swap Interval | `video_swap_interval = "1"` | Set `2` for 30 fps on 60 Hz display |
| VSync | `video_vsync = "true"` | Drift-guard pin |
| Refresh Rate | `video_refresh_rate = "59.940060"` | NTSC seed; calibrate via Settings → Video → Output |

### Latency reduction

| Setting | Value | Notes |
|---------|-------|-------|
| Sync to Exact Content Framerate | `vrr_runloop_enable = "false"` | Fixed-refresh ATV; keeps DRC active |
| Run Ahead | `run_ahead_enabled = "false"`, `run_ahead_frames = "2"` | Tier 1 per-core `true`; Tier 2 explicit `false`; global frame count matches Tier 1 |
| Run-Ahead Mode | `run_ahead_secondary_instance = "false"` | Single instance, ~½ CPU cost; Tier 1 frames=2; flip to `true` per-core for crackle |
| Preemptive Frames | `preemptive_frames_enable = "false"` | RA v1.15.0 ([PR #14832](https://github.com/libretro/RetroArch/pull/14832)); per-core only; exclusive with `run_ahead_enabled` |
| Automatic Frame Delay | `video_frame_delay_auto = "true"` | Mupen pinned `false` ([#14201](https://github.com/libretro/RetroArch/issues/14201)) |
| Static Frame Delay | `video_frame_delay = "0"` | Test nonzero per-core |
| Frame Rest | `video_frame_rest = "1"` | v3.5; RA 1.17 CPU end-of-frame sleep; 1–3 ms CPU→display; fixed-refresh only |
| Input Poll Mode | `input_poll_type_behavior = "2"` | Late sampling; ~0.5–1 frame reduction; netplay forces `0` |
| Fast Forward Ratio | `fastforward_ratio = "4.0"` | v3.6 standard cap (was `"5.0"` in v3.3–v3.5, uncapped `"0.0"` pre-v3.3); 4× matches community standalone-emulator norms and reduces thermal load during sustained FF bursts on passive A15 |
| Fast Forward Frameskip | `fastforward_frameskip = "true"` | v3.5; drops GPU ~50% when FF engages; inert at 1× |

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

The companion `retroarch.cfg` includes hardening, input, menu performance, and logging settings. Command and remote-control interfaces are explicitly disabled (`stdin_cmd_enable`, `network_cmd_enable`, `network_remote_enable` all `false` as of v3.5). tvOS-inert driver subsystems (bluetooth, wifi, midi, record, camera, location) are set to `null` to skip init.

| Category | Setting | Value | Notes |
|----------|---------|-------|-------|
| Security | stdin Command, Camera, Location | `stdin_cmd_enable = "false"`, `camera_allow = "false"`, `location_allow = "false"` | — |
| Security | Network Command Surfaces | `network_cmd_enable = "false"`, `network_remote_enable = "false"` | v3.5; explicit-off UDP command / remote-control paths (port 55355) |
| Security | On-Demand Thumbnails | `network_on_demand_thumbnails = "false"` | Hangs on slow thumbnail server ([#17242](https://github.com/libretro/RetroArch/issues/17242)) |
| Cloud | Cloud Sync | `cloud_sync_enable = "false"` | Master gate; sub-keys inherit |
| Network | Netplay Public Announce | `netplay_public_announce = "false"` | Upstream default ON |
| Network | Netplay NAT Traversal | `netplay_nat_traversal = "false"` | Blocks UPnP/NAT-PMP probing |
| Network | Netplay MITM Server | `netplay_use_mitm_server = "false"` | Drift-guard |
| Network | Netplay Start as Spectator | `netplay_start_as_spectator = "false"` | Drift-guard |
| Network | Discord RPC | `discord_allow = "false"` | — |
| Drivers | Null subsystems | `bluetooth_driver`, `wifi_driver`, `midi_driver`, `record_driver`, `camera_driver`, `location_driver` = `"null"` | v3.5; tvOS handles these at OS level — RA skip-init |
| Menu | Widgets (Animated Notifications) | `menu_enable_widgets = "false"` | Removes GPU compositing overhead |
| Menu | XMB Animations | `menu_xmb_animation_opening_main_menu`, `menu_xmb_animation_horizontal_highlight`, `menu_xmb_animation_move_up_down` = `"0"` | v3.5; instant menu cuts; drops idle-menu GPU load |
| Menu | RGUI Inline Thumbnails | `rgui_inline_thumbnails = "false"` | v3.5; cache + CPU save |
| Menu | Menu Sublabels | `menu_show_sublabels = "false"` | v3.5; complements `playlist_show_sublabels = "false"` |
| Menu | Core Info Cache | `core_info_cache_enable = "true"` | v3.5; cold-start win on 64 GB purgeable-cache |
| Video | Waitable Swapchains | `video_waitable_swapchains = "false"` | Pacing overhead unneeded on fixed-refresh tvOS |
| Video | OSD Font Rendering | `video_font_enable = "false"` | v3.5; OSD text path disabled (FPS counter / notifications no longer draw) |
| Video | Black Frame Insertion | `video_black_frame_insertion = "0"`, `video_bfi_dark_frames = "1"` | v3.5; explicit 0 for 60 Hz panel; BFI dark-frame count drift-guard |
| Video | Shader Subframes | `video_shader_subframes = "1"` | v3.5; defensive default vs future release drift |
| Video | HDR | `video_hdr_enable = "false"`, `video_hdr_max_nits = "1000"`, `video_hdr_contrast = "5.0"` | v3.5; explicit-off guard against Metal HDR10 negotiation on DV/HDR10 TV modes (see §7 TV output — keep tvOS in 4K SDR) |
| Input | Joypad Driver | `input_joypad_driver = "mfi"` | Apple GCController; only viable driver |
| Input | Menu Toggle Combo | `input_menu_toggle_gamepad_combo = "2"` | L3+R3 |
| Input | Overlay Subsystem | `input_overlay_enable = "false"` | No touch surface on tvOS |
| Input | Auto Game Focus | `input_auto_game_focus = "1"` | v3.5; auto-grabs focus on content load; prevents Siri Remote leak |
| Input | Bind Timeout | `input_bind_timeout = "3"` | v3.5; BT HID retry window; default 5 s drops GCController samples |
| Menu | Favorites / History Size | `content_favorites_size = "10"`, `content_history_size = "10"` | Default 200; reduced for 4 GB RAM |
| Menu | Pause on Menu | `menu_pause_libretro = "false"` | v3.5; was `"true"` — was neutralizing Run Ahead catch-up and FF engagement through menu open. Core keeps running behind Quick Menu |
| Menu | Menu Driver | `menu_driver = "xmb"` | Restart required to switch |
| Menu | Pause on Focus Loss | `pause_nonactive = "false"` | v3.5; was `"true"` — tvOS briefly marks app inactive on Siri Remote / Control Center / HDMI CEC, causing audio glitch |
| Menu | Playlist Sub-labels | `playlist_show_sublabels = "false"` | Prevents XMB scroll lag |
| Saving | Auto-Index States | `savestate_auto_index = "true"` | Required for slot rotation |
| Saving | Auto-Save on Exit | `savestate_auto_save = "true"` | Captures on Close Content; manual load via Select + L1 |
| Saving | Auto-Load on Launch | `savestate_auto_load = "false"` | Prevents rollback over SRAM |
| Saving | Save Config on Exit | `config_save_on_exit = "false"` | Prevents accidental overwrite |
| Saving | Max Auto-Increment States | `savestate_max_keep = "10"` | — |
| Saving | Save State Compression | `savestate_file_compression = "true"` | — |
| Saving | SaveRAM Compression | `save_file_compression = "true"` | — |
| Saving | State Thumbnails | `savestate_thumbnail_enable = "false"` | v3.1: skips PNG encode per save |
| Saving | Sort Save Files / States | `sort_savefiles_enable = "true"`, `sort_savestates_enable = "true"` | Per-core subfolders (drift-guard) |
| Logging | Verbosity / File Logging | `log_verbosity = "false"`, `log_to_file = "false"` | `os_log` per-message alloc cost |
| Audio | Audio Driver | `audio_driver = "coreaudio"` | Pinned; `coreaudio3` is master-only — do not use |
| Audio | `audio_out_rate` | `48000` Hz | Native HDMI; no resampling |
| Audio | Audio Latency | `audio_latency = "48"` | v3.6; was `"32"` in v3.5, `"64"` pre-v3.5. 48 is the sweet spot (~3 frames @ 60 Hz) — preserves most of the 32 benefit while eliminating thermal-throttle crackle risk. PCSX mirrors `48` (drift-guard); Mupen pins `64` (+16 ms for E-core + GC + scheduler variance). Revert to 64 if crackle under sustained load |
| Audio | Resampler Quality | `audio_resampler_quality = "1"` | v3.1: lowered from 2; imperceptible at 48 kHz |
| Audio | Audio Sync | `audio_sync = "true"` | Drift-guard; v3.3 Mupen pins `false` (clean dropped frames vs pitch rubber-band) |
| Video | Threaded Video | `video_threaded = "false"` | Force-disabled on all Apple platforms ([#14978](https://github.com/libretro/RetroArch/issues/14978)) |
| Video | Aspect Ratio | `aspect_ratio_index = "22"` | Core Provided |
| Menu | Playlist Compression | `playlist_compression = "true"` | ~90% reduction |
| System | Screensaver Suspend | `suspend_screensaver_enable = "true"` | tvOS may still fire while paused |
| Latency | Run Ahead Hide Warnings | `run_ahead_hide_warnings = "true"` | Per-core overrides handle incompatibility |

See also the [WebDAV security warning](#4-file-transfers) in §4.

### Netplay

RetroArch supports peer-to-peer Netplay with up to 16 players and spectators. A low-latency network is recommended (5 GHz Wi-Fi or Ethernet on the 128 GB model). Only cores with serialization (save state) support are compatible.

## 8. Shaders

### Applying a shader

> **Shader pipeline is enabled by default** (`video_shader_enable = "true"`). `retroarch.cfg` does not set a global `video_shader` preset — users select a preset per-core via Quick Menu → Shaders → Save Core Preset.

The companion `retroarch-configs` `.cfg` files likewise do not set `video_shader`. To assign a preset for a specific core:

1. Launch a game → Quick Menu (L3 + R3) → Shaders → Video Shaders: **ON**.
2. Load Preset → `shaders_slang` → `crt` (or `handheld` for LCD-style systems) → select a preset.
3. Adjust parameters as needed (see **crt-easymode 4K parameters** below if using that preset).
4. Save Preset → **Save Core Preset** (writes a per-core shader preset so it reloads automatically for that core).

> **Handheld note:** `crt-easymode` is a CRT shader. For mGBA (GBA/GB/GBC) users who prefer the LCD aesthetic, apply `handheld/lcd-grid-v2.slangp` via Save Core Preset per the steps above.

### Recommended presets

Grouped by GPU cost at 4K output on the passively-cooled A15:

| GPU Cost | Shader | Best For |
|----------|--------|----------|
| Minimal | `zfast_crt.slangp` | All systems, especially PS1/N64 |
| Minimal | `crt-pi.slangp` | Heaviest cores; comparable weight to zfast_crt |
| Minimal | `crt-potato-warm/cool.slangp` | All systems (lookup-texture based) |
| Low | `crt-easymode.slangp` | **Recommended starting point** (all cores); cleaner 4K phosphor mask than crt-aperture; integer-scale safe (no shader geometry); all 2D systems |
| Low | `crt-hyllian.slangp` | SNES, Genesis (Trinitron aesthetic) |
| Low | `crt-aperture.slangp` | Aperture-grille look; best all-rounder across 2D systems, lighter than crt-hyllian |
| Low | `fakelottes.slangp` | 16-bit systems (lighter crt-lottes) |
| Medium | `crt-geom.slangp` | All 2D systems (scanlines + curvature) |
| Medium | `crt-lottes-fast.slangp` | 16-bit systems (slot mask + bloom) |

Applying `crt-easymode.slangp` per-core (Low cost) is a safe starting point across all cores including Tier 2. If Tier 2 cores drop frames or thermally throttle, swap per-core to a Minimal preset (`zfast_crt`, `crt-pi`, `crt-potato-warm/cool`). If complex shaders cause issues at 4K globally, switch Apple TV output to 1080p SDR 60 Hz — the TV's scaler handles upscaling and GPU load drops significantly.

**Avoid on Apple TV:** CRT-Royale, CRT-Geom-Deluxe, Guest-Dr-Venom, Guest-Advanced, and all Mega Bezel shaders exceed the A15's GPU budget.

> **Integer Scaling Conflict:** Multi-pass CRT shaders that perform their own geometry (crt-geom, crt-hyllian) expect to control the full output resolution. `video_scale_integer = ON` constrains the viewport *before* the shader processes it, resulting in a smaller and potentially distorted image. Set `video_scale_integer = OFF` in per-core overrides where geometry shaders are used. Simple scanline-only shaders (zfast_crt, crt-pi) are unaffected.

**crt-easymode 4K parameters:** `crt-easymode.slangp` exposes SHARPNESS_IMAGE, SHARPNESS_EDGES, GLOW_WIDTH/HEIGHT/HALATION/DIFFUSION, MASK_COLORS, MASK_STRENGTH, MASK_SIZE, SCANLINE_SIZE_MIN/MAX, SCANLINE_SHAPE, GAMMA_INPUT/OUTPUT, and BRIGHTNESS via Quick Menu → Shaders → Shader Parameters. Starting values are reasonable out-of-the-box on a 4K display; tune to taste.

> **Note:** If `config/shaders/shaders_slang/crt/` appears empty after an update, re-run Online Updater → Update Slang Shaders. If that fails, upload presets manually via WebDAV to `config/shaders/shaders_slang/crt/`.

## 9. Supported Systems and Per-Core Overrides

The A15 Bionic handles retro emulation effectively, but Apple's App Store restriction on JIT compilation limits performance for demanding systems. Dreamcast, GameCube, Wii, and PS2 require JIT and cannot run through the App Store version.

Per-core override values and core options are maintained in the companion [retroarch-configs](https://github.com/ryanmusante/retroarch-configs) repository. That ZIP ships its files flat under `config/`; after upload, move each `.cfg` / `.opt` file into the matching `config/<core_name>/` directory on the Apple TV (see [Filesystem layout](#filesystem-layout-apple-tv)). Once placed there, both file types load automatically — no manual entry required inside RetroArch. Tier 1 `.cfg` files explicitly enable Run Ahead per core; the global baseline in `retroarch.cfg` leaves `run_ahead_enabled = "false"`.

Tier definitions: **1** = Flawless (full speed, shaders enabled), **2** = Good (most titles at full speed).

| Tier | System | Core | Override | Notes |
|------|--------|------|----------|-------|
| 1 | NES | Mesen | Yes | Overscale 224p; per-game `mesen_overclock_rate` for Battletoads, Recca |
| 1 | SNES | Snes9x | Yes | Overscale 224p; per-game `snes9x_overclock = 200` for SuperFX (Star Fox, Yoshi's Island, Doom, Stunt Race FX) |
| 1 | GB / GBC / GBA | mGBA | Yes | — |
| 1 | Genesis / MD / CD, SMS | Genesis Plus GX | Yes | Overscale 224p; Run Ahead per-core |
| 1 | PC Engine / TG-16 | Beetle PCE Fast | Yes | — |
| 1 | Neo Geo, Arcade (CPS1/2/3) | FinalBurn Neo | Yes | Rewind pinned `false` ([#16374](https://github.com/libretro/RetroArch/issues/16374)) |
| 2 | PlayStation 1 | PCSX-ReARMed | Yes | No JIT; Run Ahead `false`; `pcsx_rearmed_psxclock="100"` (underclock 75/50 per-game for Tony Hawk, Spyro 2/3, Tekken 3); v3.3 pins `video_max_swapchain_images=3` + `autosave_interval=0` |
| 2 | Nintendo 64 | Mupen64Plus-Next | Yes | Angrylion + CXD4 (platform-forced on Metal build). v3.3 `mupen64plus-angrylion-multithread = "2"` (P-core pin on 2P+4E A15). Pins: `video_threaded=false` ([#14978](https://github.com/libretro/RetroArch/issues/14978)), `video_frame_delay_auto=false` ([#14201](https://github.com/libretro/RetroArch/issues/14201)), `rewind_enable=false` ([#18300](https://github.com/libretro/RetroArch/issues/18300)), `run_ahead_enabled=false`, `audio_latency=96`, `video_max_swapchain_images=3`, `audio_sync=false`, `autosave_interval=0` |

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

- [ ] Shader pipeline enabled (`video_shader_enable = "true"`); no global preset set in `retroarch.cfg` — select per-core via Quick Menu → Shaders → Save Core Preset

### Per-core overrides

- [ ] Tier 1–2 override files (`.cfg` and `.opt`) uploaded to `config/<core_name>/` (see §9)
- [ ] Both file types load automatically — verify via Quick Menu → Information

## 12. Files in This Repository

| File | Description |
|------|-------------|
| `README.md` | This guide |
| [`CHANGELOG.md`](CHANGELOG.md) | Release history (GNU ChangeLog style) |
| `retroarch.cfg` | Drop-in configuration for Apple TV 4K 3rd Gen |
| `LICENSE` | MIT License |

## 13. Versioning

This repository uses `vMAJOR.MINOR` (no patch component) in lockstep with its companion repository — both `retroarch-appletv4k` and `retroarch-configs` share a single MAJOR.MINOR tag that increments together on every release, regardless of which side contains the real file changes. `MAJOR` increments on incompatible structural changes (filesystem layout, breaking config schema, removed features) or cross-repo version-sync events (e.g. v3.0 re-aligned the two repos after they had evolved independently at v2.95 / v1.57). `MINOR` increments on every release — additive changes, key additions/removals, documentation syncs, and conservative defaults adjustments. Each release ships with a matching `CHANGELOG.md` entry in GNU ChangeLog style (`YYYY-MM-DD<SP><SP>Author Name`; date and author separated by two spaces). `CHANGELOG.md` retains the last 5 MINOR version entries; older entries are trimmed on each release.

## 14. License

[MIT](LICENSE)

RetroArch is a separate project licensed under GPL v3. This guide and configuration file are independent community resources.
