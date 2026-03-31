# RetroArch on Apple TV 4K

**RetroArch v1.22.x** · **tvOS 18+** · **Apple TV 4K 3rd Gen (64 GB Wi-Fi · j255ap · A2737)** · **March 2026** · **Rev. 9**

Turn your Apple TV into a retro gaming console. This guide covers installation, ROM and BIOS setup, controller configuration, performance tuning, and CRT shaders for the Apple TV 4K 3rd Generation. A companion `retroarch.cfg` with all recommended settings is included.

### Quick Start

1. Install RetroArch from the tvOS App Store.
2. Run Online Updater (Assets, Core Info, Databases, Slang Shaders).
3. Upload `retroarch.cfg`, ROMs, and BIOS files via the web interface or WebDAV.
4. Pair a Bluetooth controller and configure hotkeys.
5. Scan your ROMs (Manual Scan) and play.

For detailed instructions, continue reading below.

---

## Table of Contents

1. [Prerequisites](#1-prerequisites)
2. [Installation](#2-installation)
3. [File Transfers](#3-file-transfers)
4. [ROM and BIOS Setup](#4-rom-and-bios-setup)
5. [Storage Persistence](#5-storage-persistence)
6. [Controllers](#6-controllers)
7. [Configuration](#7-configuration)
8. [CRT Shaders](#8-crt-shaders)
9. [iCloud Sync](#9-icloud-sync)
10. [Per-Core Overrides](#10-per-core-overrides)
11. [Supported Systems](#11-supported-systems)
12. [Known Issues](#12-known-issues)
13. [Setup Checklist](#13-setup-checklist)
14. [Appendix A: 4th Gen Projections](#appendix-a-4th-gen-projections)

---

## 1. Prerequisites

Minimum recommended version: **RetroArch v1.20.0** (required for WebDAV, automatic frame delay, and integer scaling enhancements). Versions before v1.19.1 lack automatic config backup and asset re-extraction.

### Hardware

| Component | Specification | Notes |
|-----------|--------------|-------|
| Apple TV | 4K 3rd Gen (2022), 64 GB Wi-Fi (j255ap / A2737) | A15 Bionic; 64 GB — tvOS purges cache sooner than 128 GB model |
| Controller | PS5 DualSense or Xbox Series X/S | Any Bluetooth gamepad; Siri Remote is menu-only |
| Network | Same Wi-Fi/LAN for Apple TV and computer | Wi-Fi only (no Ethernet on 64 GB model) |
| Computer | Mac, Windows, or Linux with a browser | Used for ROM/BIOS file transfers |

### Software

| Software | Details |
|----------|---------|
| tvOS | Latest version installed |
| RetroArch | Free from the tvOS App Store |
| Web browser | Safari, Chrome, Firefox, or Edge on your computer |
| ROM files | Legally acquired game files for systems you own |
| BIOS files | Required for PS1, Sega CD, Saturn, Neo Geo (see [ROM and BIOS Setup](#4-rom-and-bios-setup)) |

> **Note:** Only use ROM and BIOS files for games and hardware you legally own. Downloading copyrighted material you do not own is illegal in most jurisdictions.

---

## 2. Installation

No sideloading, developer accounts, or jailbreaking required.

1. **App Store:** Search for "RetroArch" (Daniel De Matteis) → tap Get.
2. **Launch:** A Welcome popup displays two URLs (local IP and Bonjour hostname). Write them down — you will need these for file transfers.
3. **Online Updater** (Main Menu → Online Updater):
   - Update Assets (icons, fonts, overlays)
   - Update Core Info Files
   - Update Databases
   - Update Slang Shaders
4. **Apply configuration:** Upload the companion `retroarch.cfg` via the web interface (see [File Transfers](#3-file-transfers)), then quit and relaunch RetroArch. Do **not** use "Save Current Configuration" before relaunching — it overwrites the uploaded file with the old in-memory config.

> **Tip:** RetroArch v1.20.0 added Bluetooth keyboard support on tvOS for text input, search, and core option editing.

---

## 3. File Transfers

tvOS has no Files app. All transfers use RetroArch's built-in network servers. RetroArch must remain running during transfers.

### Web interface

1. Ensure your computer and Apple TV are on the same network.
2. Browse to the URL from the Welcome popup (e.g., `http://192.168.x.x` or `http://appletv.local`).
3. Navigate into the `Config` folder.
4. Create `ROMs` and `BIOS` folders with subfolders per system (see [ROM and BIOS Setup](#4-rom-and-bios-setup)).
5. Drag and drop files into the appropriate subfolder.

### WebDAV (bulk transfers)

Available in RetroArch v1.20.0+. Port 8080.

| Platform | Connection |
|----------|-----------|
| macOS | Finder → Go → Connect to Server (⌘K) → `http://appletv.local:8080` |
| Windows | File Explorer → right-click This PC → Map Network Drive → `http://appletv.local:8080` |

> **Note:** The 64 GB Wi-Fi model has no Ethernet port. Use 5 GHz Wi-Fi for faster transfers and keep the Apple TV close to your router.

---

## 4. ROM and BIOS Setup

### ROM folder reference

Place ROMs in `Config/ROMs/<folder>/` using the folder names below. These names match RetroArch's database scanner and are used for Manual Scan.

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
| PC Engine / TG-16 | `pce/` | `.pce`, `.cue`, `.chd` | Beetle PCE | 1 |
| Neo Geo | `neogeo/` | `.zip` | FinalBurn Neo | 1 |
| Arcade (CPS1/2/3) | `fbneo/` | `.zip` | FinalBurn Neo | 1 |
| PlayStation 1 | `psx/` | `.cue`, `.bin`, `.chd`, `.pbp` | PCSX ReARMed | 2 |
| Nintendo 64 | `n64/` | `.n64`, `.z64`, `.v64` | Mupen64Plus-Next | 2 |
| Nintendo DS | `nds/` | `.nds` | melonDS DS | 2 |
| Sega Saturn | `saturn/` | `.cue`, `.chd` | Yabause | 3 |
| PSP | `psp/` | `.iso`, `.cso` | PPSSPP | 3 |
| Nintendo 3DS | `3ds/` | `.3ds`, `.cia` | Azahar | 3 |

Tier definitions: **1** = Flawless (full speed, shaders OK), **2** = Good (most titles full speed), **3** = Limited/Experimental. See [Supported Systems](#11-supported-systems) for details.

### BIOS files

Most 8/16-bit cores have built-in BIOS support. The following 32-bit+ systems require external BIOS files placed in `Config/BIOS/`.

| System | Required File(s) | Required? |
|--------|-----------------|-----------|
| PlayStation 1 | `scph5501.bin` (NA) or `scph1001.bin` | Yes |
| Sega CD / Mega CD | `bios_CD_U.bin`, `bios_CD_E.bin`, `bios_CD_J.bin` | Yes |
| Sega Saturn | `sega_101.bin`, `mpr-17933.bin` | Yes |
| TurboGrafx-CD | `syscard3.pce` | Yes |
| Neo Geo | `neogeo.zip` | Yes |
| Nintendo DS | `bios7.bin`, `bios9.bin`, `firmware.bin` | Yes |
| Game Boy Advance | `gba_bios.bin` | Optional |

Neo Geo requires `neogeo.zip` in **both** `Config/BIOS/` and `Config/ROMs/neogeo/`. BIOS filenames are case-sensitive — use exact names (e.g., `scph5501.bin`, not `SCPH5501.BIN`).

### Scanning games

Use Manual Scan to build playlists:

1. Main Menu → Import Content → Manual Scan.
2. **Content Directory:** Browse to your ROMs folder, select a system subfolder.
3. **System Name:** Choose the matching platform.
4. **Default Core:** Select the emulation core.
5. **Start Scan.** Repeat for each system.

Playlists appear under Main Menu → Playlists.

### Directory structure

```
Config/
├── BIOS/
│   ├── scph5501.bin          # PS1
│   ├── neogeo.zip            # Neo Geo
│   ├── bios7.bin             # NDS
│   ├── bios9.bin             # NDS
│   └── firmware.bin          # NDS
├── ROMs/
│   ├── nes/
│   ├── snes/
│   ├── gb/
│   ├── gbc/
│   ├── gba/
│   ├── megadrive/
│   ├── segacd/
│   ├── mastersystem/
│   ├── pce/
│   ├── neogeo/
│   │   └── neogeo.zip        # Also in BIOS/
│   ├── fbneo/
│   ├── psx/
│   ├── n64/
│   ├── nds/
│   ├── saturn/
│   ├── psp/
│   └── 3ds/
└── retroarch.cfg
```

---

## 5. Storage Persistence

> ⚠️ **Warning:** This is the most important section. Understanding tvOS storage behavior prevents data loss.

tvOS guarantees only **500 KB** of persistent storage per app. Everything else — ROMs, saves, BIOS, shaders — is purgeable cache that tvOS silently deletes when storage runs low. The 64 GB model is more aggressive about cache purges than the 128 GB model.

### Automatic config backup

Since v1.19.1, RetroArch stores `retroarch.cfg` in NSUserDefaults (the 500 KB persistent area). Shader assets are re-extracted automatically when detected missing.

### Recommended setup

1. Create `ROMs/` and `BIOS/` folders inside `Config/`.
2. Upload ROM and BIOS files into appropriate subfolders.
3. Set **File Browser** to `Config/ROMs/` (Settings → Directory).
4. Set **System/BIOS** to `Config/BIOS/` (Settings → Directory).
5. Enable **iCloud Cloud Sync:** Settings → Saving → Cloud Sync → Enable → Backend: iCloud.

ROM files are never synced by iCloud. Always maintain backups on your computer. Keep your ROM library lean to reduce cache pressure on the 64 GB model.

---

## 6. Controllers

The Apple TV supports up to four simultaneous Bluetooth controllers. The Siri Remote navigates menus only.

### Pairing

1. **Apple TV:** Settings → Remotes and Devices → Bluetooth.
2. Put the controller into pairing mode (see table below).
3. Select the controller under "Other Devices."
4. Launch RetroArch — the controller is recognized automatically.

### Compatibility

| Controller | Pairing Method | Status |
|-----------|---------------|--------|
| PS5 DualSense / Edge | PS + Create buttons | **Recommended** — full support, BT 5.1 low latency |
| Xbox Series X/S Wireless | Xbox + Connect button | **Recommended** — full support |
| PS4 DualShock 4 | PS + Share buttons | Excellent — most stable |
| 8BitDo Pro 2 / Ultimate | Mode D/A + Pair button | Excellent — multi-platform |
| SteelSeries Nimbus+ | Bluetooth button | Good — MFi certified |
| Nintendo Switch Pro | Sync button | **Avoid** — B button exits app (see [Known Issues](#12-known-issues)) |

> **Note:** Ghost inputs from controllers 2+ may bleed into controller 1 in multi-controller setups. Test before relying on multiplayer (see [Known Issues](#12-known-issues)).

---

## 7. Configuration

### User interface

RetroArch defaults to Ozone. For a TV-friendly console-style crossbar interface, change to XMB:

1. Settings → Drivers → Menu → **XMB**.
2. Quit and relaunch RetroArch.

### Hotkeys

The PS/Xbox home button opens tvOS Control Center, not RetroArch's menu. A controller combo is required.

**Menu toggle:** Settings → Input → Hotkeys → Menu Toggle Controller Combo → **L3 + R3** (click both thumbsticks). Optionally set an **Enable Hotkeys** modifier (e.g., Select/Share).

**Recommended bindings** (with Select as the Enable Hotkeys modifier):

| Action | Combo |
|--------|-------|
| Open Quick Menu | L3 + R3 |
| Save State | Select + R1 |
| Load State | Select + L1 |
| Fast Forward | Select + R2 (hold) |
| Rewind | Select + L2 (hold) |
| State Slot + | Select + D-Pad Right |
| State Slot − | Select + D-Pad Left |
| Close Content | Select + Start |

> ⚠️ **Warning:** Without Close Content configured, there is no way to exit a running game without force-quitting the app.

### Video settings

| Setting | Value | Notes |
|---------|-------|-------|
| Video driver | Metal | Best performance on Apple silicon |
| V-Sync | ON | — |
| Integer Scale | ON | Pixel-perfect output; produces borders at 4K |
| Integer Overscale | Optional | Enable for 224p content (NES/SNES) to fill more screen |
| Bilinear Filtering | OFF | Required for correct shader rendering |
| Threaded Video | Omitted | Crashes on tvOS (see [Known Issues](#12-known-issues)) |
| Frame Rest | ON | Sleeps GPU between frames; reduces heat/power (v1.17.0+) |
| Max Swapchain Images | 2 | Verify in Settings → Video → Synchronization |
| Aspect Ratio | Core Provided | — |

### Latency reduction

| Setting | Value | Notes |
|---------|-------|-------|
| Preemptive Frames | ON, 1 frame | Lower-cost run-ahead method (v1.16.0+); `run_ahead_frames` sets count |
| Automatic Frame Delay | ON | Disable per-core for N64 (see [Per-Core Overrides](#10-per-core-overrides)) |
| Input Poll Type Behavior | Late | — |

### TV output

| Setting | Location | Value |
|---------|----------|-------|
| Resolution | tvOS Settings → Video and Audio | 4K SDR 60 Hz |
| Match Frame Rate | tvOS Settings → Video and Audio → Match Content | ON |
| Match Dynamic Range | tvOS Settings → Video and Audio → Match Content | OFF |
| Audio Format | tvOS Settings → Video and Audio | Stereo |
| Reduce Loud Sounds | tvOS Settings → Video and Audio | OFF |
| Game Mode | TV settings (HDMI input) | ON |
| Chroma subsampling | TV settings (HDMI input) | YCbCr 4:4:4 or RGB Full |

Apple TV does not support gaming VRR — only QMS VRR for media. Leave "Sync to Exact Content Framerate" OFF.

### Netplay

RetroArch supports peer-to-peer Netplay with up to 16 players and spectators. Low-latency network recommended (5 GHz Wi-Fi or Ethernet on 128 GB model). Only cores with serialization (save state) support are compatible.

---

## 8. CRT Shaders

CRT shaders simulate scanlines, phosphor glow, and curvature for authentic visuals.

### Applying a shader

1. Launch a game → Quick Menu (L3 + R3) → Shaders → Video Shaders: **ON**.
2. Load Preset → `shaders_slang` → `crt` → choose a preset.
3. Save Preset → **Save Core Preset** (applies to all games on that core).

### Recommended presets

| Shader | Best For | GPU Load |
|--------|----------|----------|
| `zfast_crt.slangp` | All systems, especially PS1/N64/Saturn | Minimal |
| `crt-easymode.slangp` | NES, SNES, Genesis, GBA | Low |
| `crt-hyllian.slangp` | SNES, Genesis (Trinitron look) | Low |
| `crt-geom.slangp` | All 2D systems (scanlines + curvature) | Low–Moderate |
| `crt-lottes-fast.slangp` | 16-bit systems (slot mask + bloom) | Moderate |
| `crt-pi.slangp` | Heaviest cores; lighter than zfast_crt | Minimal |
| `crt-potato-warm/cool.slangp` | All systems (lookup-texture based) | Minimal |
| `fakelottes.slangp` | 16-bit systems (lighter crt-lottes) | Low |
| `crt-aperture.slangp` | SNES, Genesis (lighter than crt-hyllian) | Low |

**Avoid on Apple TV:** CRT-Royale, CRT-Geom-Deluxe, Guest-Dr-Venom, Guest-Advanced, and all Mega Bezel shaders — too GPU-intensive for the A15.

**crt-easymode 4K parameters** (community-recommended starting point): Mask Strength 0.18, Mask Type 1, Scanline Strength 0.95, Gamma Input 2.2, Gamma Output 1.8, Brightness 1.10. Adjust Mask Strength to taste on your display.

> **Tip:** If `shaders_slang/crt/` appears empty after an update, re-run Online Updater → Update Slang Shaders. If that fails, manually upload presets via WebDAV to `Config/shaders_slang/crt/`.

---

## 9. iCloud Sync

Save on one Apple device, continue on another.

### Setup

1. Install RetroArch on each device with the same Apple ID.
2. Settings → Saving → Cloud Sync → Enable → **ON**.
3. Backend: **iCloud**.
4. Ensure "Sort Saves into Folders by Core Name" is consistent across devices.

### What syncs

| Data | Syncs? | Notes |
|------|--------|-------|
| Save files (.srm) | Yes | In-game saves |
| Save states | Yes | Quick save/load |
| Configuration | Yes | Core settings, hotkeys, directories |
| Thumbnails | Optional | Consumes iCloud quota |
| BIOS files | Optional | Enable system file sync |
| ROM files | **No** | Keep backups on your computer |

`retroarch.cfg` does **not** sync via Cloud Sync — upload manually to each device. The iCloud Drive backend (PR #17814) is available on macOS/iOS but not Apple TV; continue using the CloudKit backend.

> **Note:** Close content and return to the main menu before quitting RetroArch to avoid sync conflicts between devices (see [Known Issues](#12-known-issues)).

---

## 10. Per-Core Overrides

Override files are stored at `Config/config/<core_name>/<core_name>.cfg`. Create them via Quick Menu → Overrides → Save Core Overrides.

### Tier 1 cores (rewind + run-ahead safe)

| Core | Overrides |
|------|-----------|
| **Mesen** (NES) | `rewind_enable = "true"`, `run_ahead_frames = "1"` |
| **Snes9x** (SNES) | `rewind_enable = "true"`, `run_ahead_frames = "1"` |
| **mGBA** (GBA) | `rewind_enable = "true"`, `run_ahead_frames = "1"` |
| **Genesis Plus GX** | `rewind_enable = "true"`, `run_ahead_frames = "1"` |
| **Beetle PCE** | `rewind_enable = "true"`, `run_ahead_frames = "1"` |
| **FinalBurn Neo** | `rewind_enable = "true"`, `run_ahead_frames = "1"` |

Snes9x run-ahead can be bumped to 2 for SMW/Zelda if no audio artifacts are observed.

### Tier 2 cores (limited latency reduction)

| Core | Overrides | Notes |
|------|-----------|-------|
| **PCSX ReARMed** (PS1) | `run_ahead_frames = "1"`, `rewind_enable = "false"` | — |
| **Mupen64Plus-Next** (N64) | `run_ahead_frames = "1"`, `rewind_enable = "false"`, `video_frame_delay_auto = "false"`, `audio_latency = "64"` | Rewind freezes emulation; auto frame delay incompatible (#14201) |
| **melonDS DS** (NDS) | `run_ahead_frames = "1"`, `rewind_enable = "false"` | Use software renderer at 1x resolution |

### Tier 3 cores (disable all latency features)

| Core | Overrides | Notes |
|------|-----------|-------|
| **Yabause** (Saturn) | `preemptive_frames_enable = "false"`, `run_ahead_frames = "0"`, `rewind_enable = "false"`, `video_shader_enable = "false"` | — |
| **PPSSPP** (PSP) | `video_driver = "gl"`, `preemptive_frames_enable = "false"`, `run_ahead_frames = "0"`, `rewind_enable = "false"`, `video_shader_enable = "false"` | Metal/Vulkan crashes; set core options: `ppsspp_vertex_cache = enabled`, `ppsspp_separate_io_thread = enabled`, `internal_resolution = 1x` |
| **Azahar** (3DS) | `preemptive_frames_enable = "false"`, `run_ahead_frames = "0"`, `rewind_enable = "false"`, `video_shader_enable = "false"` | — |

---

## 11. Supported Systems

The A15 Bionic handles retro emulation well, but Apple's App Store restriction on JIT compilation limits performance for demanding systems. Dreamcast, GameCube, Wii, and PS2 require JIT and cannot run through the App Store version.

### Tier 1 — Flawless

Full speed with CRT shaders enabled. No per-core workarounds needed.

| System | Core |
|--------|------|
| NES | Mesen |
| SNES | Snes9x |
| GB / GBC / GBA | mGBA |
| Genesis / MD / CD, Master System | Genesis Plus GX |
| PC Engine / TG-16 | Beetle PCE |
| Neo Geo, Arcade (CPS1/2/3) | FinalBurn Neo |

### Tier 2 — Good

Most titles at full speed. Some per-core overrides recommended.

| System | Core | Notes |
|--------|------|-------|
| PlayStation 1 | PCSX ReARMed | Run-ahead 1 safe on most titles |
| Nintendo 64 | Mupen64Plus-Next | ~60–70% compatibility; complex titles (Conker, Rogue Squadron) may have issues; disable rewind and auto frame delay |
| Nintendo DS | melonDS DS | No touchscreen input available |

### Tier 3 — Limited / Experimental

Significant compatibility or performance issues. Disable all latency features.

| System | Core | Notes |
|--------|------|-------|
| Sega Saturn | Yabause | Significant compatibility issues |
| PSP | PPSSPP | Interpreter only; must use GL driver (Metal/Vulkan crash); may crash |
| Nintendo 3DS | Azahar | Added experimentally in v1.22.x nightly builds (2026); Azahar 2125.0 at RC1 as of March 2026 |

---

## 12. Known Issues

| # | Issue | Ref | Status | Workaround |
|---|-------|-----|--------|------------|
| 1 | PPSSPP crashes with Vulkan/Metal driver | [#18050](https://github.com/libretro/RetroArch/issues/18050) | Open | `video_driver = "gl"` per-core override |
| 2 | Switch Pro B button exits app | [#18286](https://github.com/libretro/RetroArch/issues/18286) | Open | Avoid Switch Pro Controller |
| 3 | Ghost inputs with multiple controllers | [#18447](https://github.com/libretro/RetroArch/issues/18447) | Open | Use single controller or test carefully |
| 4 | iCloud Sync crash on tvOS 18.6 | — | Reports | Disable Cloud Sync temporarily |
| 5 | Mupen64Plus-Next per-core rewind feature request | [#18300](https://github.com/libretro/RetroArch/issues/18300) | Open | Rewind not safe on N64; disable per-core |
| 6 | Mupen64Plus-Next auto frame delay incompatible | [#14201](https://github.com/libretro/RetroArch/issues/14201) | Open | Disable auto frame delay per-core; refactored in v1.20.0 |
| 7 | N64 rendering glitches (game-specific) | [#16598](https://github.com/libretro/RetroArch/issues/16598) | Open | Per-game overrides |
| 8 | Threaded video crashes RetroArch on tvOS | [#14978](https://github.com/libretro/RetroArch/issues/14978) | Persists | Keep `video_threaded` omitted; upstream fix targets Mesa/GL only, not Metal |
| 9 | Cloud Sync conflicts between tvOS and macOS | [#16727](https://github.com/libretro/RetroArch/issues/16727) | Partial | DS_Store filter + foreground re-sync added; close content before quitting |
| 10 | Bluetooth controller jitter over HDMI | — | Reports | Replace HDMI cable if input latency is inconsistent |

---

## 13. Setup Checklist

### Install and configure

- [ ] RetroArch installed from tvOS App Store
- [ ] Online Updater run: Assets, Core Info, Databases, Slang Shaders
- [ ] `retroarch.cfg` uploaded via web/WebDAV and loaded on relaunch (do **not** Save Current Config first)

### Controllers and input

- [ ] Bluetooth controller paired (DualSense or Xbox Series recommended)
- [ ] Menu Toggle verified (L3 + R3 opens Quick Menu in-game)
- [ ] Hotkeys configured (Settings → Input → Hotkeys)
- [ ] Close Content hotkey set (Select + Start)

### Content

- [ ] ROMs placed in `Config/ROMs/<system>/` (see [ROM folder reference](#rom-folder-reference))
- [ ] BIOS files placed in `Config/BIOS/` (case-sensitive filenames)
- [ ] Manual Scan completed per system

### Display and performance

- [ ] Apple TV output: 4K SDR 60 Hz
- [ ] Match Dynamic Range: OFF
- [ ] tvOS Audio Format: Stereo
- [ ] tvOS Reduce Loud Sounds: OFF
- [ ] TV Game Mode enabled
- [ ] Chroma 4:4:4 / RGB Full enabled on TV
- [ ] `aspect_ratio_index` verified as Core Provided
- [ ] Automatic Frame Delay enabled
- [ ] Frame Rest enabled
- [ ] Input Poll Type Behavior set to Late
- [ ] Max Swapchain Images set to 2 (verify on Metal)

### Shaders and sync

- [ ] CRT shader applied per-core (Quick Menu → Shaders → Save Core Preset)
- [ ] iCloud Cloud Sync enabled
- [ ] Quit and relaunch to trigger initial iCloud sync

### Per-core overrides

- [ ] N64: disable auto frame delay + rewind
- [ ] PSP: switch to GL, disable latency features, set core options (vertex cache, IO thread, 1x res)

---

## Appendix A: 4th Gen Projections

> **Note:** Based on rumoured specs as of March 2026. Apple has not announced this product. Projections assume the App Store JIT restriction persists.

### Hardware comparison

| Spec | 3rd Gen (2022) | 4th Gen (rumoured) |
|------|---------------|-------------------|
| SoC | A15 Bionic (5 nm, 5-core CPU) | A17 Pro (3 nm, 6-core CPU) |
| GPU | 4-core, no hardware RT | 6-core, hardware ray tracing |
| RAM | 4 GB | 8 GB |
| Wireless | Wi-Fi 6, BT 5.0 | Wi-Fi 7, BT 6 (Apple N1) |
| Process | 5 nm (TSMC N5P) | 3 nm (TSMC N3B) |

### Projected tier changes

| System | Current | Projected | Notes |
|--------|---------|-----------|-------|
| PSP | Tier 3 | Tier 2 | ~30% CPU gain; most 2D/RPG titles playable |
| 3DS | Experimental | Tier 3 | Lighter titles near full speed |
| N64 | ~60–70% compat | ~75–85% compat | Complex titles still limited without JIT |
| PS1 | Most titles | All titles | Run-ahead 2 safe globally |
| Dreamcast/GC/Wii/PS2 | Blocked | Blocked | JIT restriction, not hardware |

### Constraints that do not change

| Constraint | Reason |
|-----------|--------|
| App Store JIT restriction | Apple policy, not hardware |
| 500 KB persistent storage | tvOS platform limit |
| No VRR for gaming | QMS only (media frame-rate switching) |
| Siri Remote not a gamepad | Hardware limitation |

---

## Files in This Repository

| File | Description |
|------|-------------|
| `README.md` | This guide |
| `CHANGELOG` | Release history (kernel.org style) |
| `retroarch.cfg` | Drop-in configuration for Apple TV 4K 3rd Gen |
| `LICENSE` | MIT License |

---

## License

This project is licensed under the [MIT License](LICENSE).

RetroArch is a separate project licensed under GPL v3. This guide and configuration file are independent community resources.
