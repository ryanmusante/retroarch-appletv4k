# RetroArch on Apple TV 4K

> The complete setup guide — turn your Apple TV into a retro gaming console.

**RetroArch v1.22.x** · **tvOS 18+** · **Apple TV 4K (3rd Generation)** · March 2026 · Rev. 2

---

## Table of Contents

1. [Prerequisites](#1-prerequisites)
2. [Supported Systems and Performance Tiers](#2-supported-systems-and-performance-tiers)
3. [ROM Folder Reference](#3-rom-folder-reference)
4. [Controller Setup](#4-controller-setup)
5. [Installing RetroArch](#5-installing-retroarch)
6. [Transferring ROM Files](#6-transferring-rom-files)
7. [BIOS Files](#7-bios-files)
8. [Scanning and Loading Games](#8-scanning-and-loading-games)
9. [Storage Persistence (Critical)](#9-storage-persistence-critical)
10. [User Interface Configuration](#10-user-interface-configuration)
11. [Hotkey Configuration](#11-hotkey-configuration)
12. [CRT Shaders](#12-crt-shaders)
13. [Cross-Device iCloud Sync](#13-cross-device-icloud-sync)
14. [Performance Optimization](#14-performance-optimization)
15. [Per-Core Override Reference](#15-per-core-override-reference)
16. [Apple TV 4K 4th Gen (2026) — Projected Impact](#16-apple-tv-4k-4th-gen-2026--projected-impact)
17. [Known Issues](#17-known-issues)
18. [Quick Reference: Setup Checklist](#18-quick-reference-setup-checklist)

---

## 1. Prerequisites

Minimum recommended version: **RetroArch v1.20.0** (required for WebDAV, automatic frame delay, integer scaling enhancements). Versions before v1.19.1 lack automatic config backup and asset re-extraction.

### Hardware

| Component | Recommendation | Notes |
|-----------|---------------|-------|
| Apple TV | 4K 3rd Gen (2022), 128 GB | A15 Bionic; 128 GB delays cache purges |
| Controller | PS5 DualSense or Xbox Series X/S | Any Bluetooth gamepad; Siri Remote is menu-only |
| Network | Same Wi-Fi/LAN for Apple TV and computer | Ethernet on 128 GB model preferred |
| Computer | Mac, Windows, or Linux with a browser | Used for ROM/BIOS file transfers |

### Software

| Software | Details |
|----------|---------|
| tvOS | Latest version installed |
| RetroArch | Free from the tvOS App Store |
| Web browser | Safari, Chrome, Firefox, or Edge on your computer |
| ROM files | Legally acquired game files for systems you own |
| BIOS files | Required for PS1, Sega CD, Saturn, Neo Geo (see [BIOS Files](#7-bios-files)) |

> **Legal:** Only use ROM and BIOS files for games and hardware you legally own. Downloading copyrighted material you do not own is illegal in most jurisdictions.

---

## 2. Supported Systems and Performance Tiers

The A15 Bionic handles retro emulation well, but Apple's App Store restriction on JIT compilation limits performance for demanding systems.

| Tier | Systems | Performance | Recommended Core |
|------|---------|-------------|-----------------|
| **1 — Flawless** | NES | Full speed, shaders OK | Mesen |
| **1 — Flawless** | SNES | Full speed, shaders OK | Snes9x |
| **1 — Flawless** | GB / GBC / GBA | Full speed, shaders OK | mGBA |
| **1 — Flawless** | Genesis / MD / CD, Master System | Full speed, shaders OK | Genesis Plus GX |
| **1 — Flawless** | PC Engine / TG-16 | Full speed, shaders OK | Beetle PCE |
| **1 — Flawless** | Neo Geo, Arcade | Full speed, shaders OK | FinalBurn Neo |
| **2 — Good** | PlayStation 1 | Most titles full speed | PCSX ReARMed |
| **2 — Good** | Nintendo 64 | ~60–70% compatibility | Mupen64Plus-Next |
| **2 — Good** | Nintendo DS | Works but no touchscreen | melonDS DS |
| **3 — Limited** | Sega Saturn | Significant compatibility issues | Yabause |
| **3 — Limited** | PSP | Interpreter only; may crash | PPSSPP |
| **3 — Experimental** | Nintendo 3DS | Below 100% on demanding titles | Azahar |

> **PSP Warning:** Crashes with Vulkan/Metal driver on tvOS (GitHub [#18050](https://github.com/libretro/RetroArch/issues/18050)). Set `video_driver = "gl"` in a per-core override. See [Per-Core Overrides](#15-per-core-override-reference).

> **Not available via App Store:** Dreamcast, GameCube, Wii, and PS2 require JIT and cannot run through the App Store version. 3DS was added experimentally in v1.22.x nightly builds (2026) via the Azahar core (Citra fork). Azahar 2125.0 is at RC1 as of March 2026.

---

## 3. ROM Folder Reference

Place ROMs in `Config/ROMs/<folder>/` using the folder names below. These folder names match RetroArch's database scanner and are used for [Manual Scan](#8-scanning-and-loading-games).

| System | ROM Folder | Extensions | Core | Tier |
|--------|-----------|------------|------|------|
| NES | `nes/` | `.nes`, `.unf`, `.unif` | Mesen | 1 — Flawless |
| SNES | `snes/` | `.sfc`, `.smc` | Snes9x | 1 — Flawless |
| Game Boy | `gb/` | `.gb` | mGBA | 1 — Flawless |
| Game Boy Color | `gbc/` | `.gbc` | mGBA | 1 — Flawless |
| Game Boy Advance | `gba/` | `.gba` | mGBA | 1 — Flawless |
| Genesis / Mega Drive | `megadrive/` | `.md`, `.gen`, `.bin` | Genesis Plus GX | 1 — Flawless |
| Sega CD / Mega CD | `segacd/` | `.cue`, `.chd` | Genesis Plus GX | 1 — Flawless |
| Master System | `mastersystem/` | `.sms` | Genesis Plus GX | 1 — Flawless |
| PC Engine / TG-16 | `pce/` | `.pce`, `.cue`, `.chd` | Beetle PCE | 1 — Flawless |
| Neo Geo | `neogeo/` | `.zip` | FinalBurn Neo | 1 — Flawless |
| Arcade (CPS1/2/3, etc.) | `fbneo/` | `.zip` | FinalBurn Neo | 1 — Flawless |
| PlayStation 1 | `psx/` | `.cue`, `.bin`, `.chd`, `.pbp` | PCSX ReARMed | 2 — Good |
| **Nintendo 64** | **`n64/`** | **`.n64`, `.z64`, `.v64`** | **Mupen64Plus-Next** | **2 — Good** |
| Nintendo DS | `nds/` | `.nds` | melonDS DS | 2 — Good |
| Sega Saturn | `saturn/` | `.cue`, `.chd` | Yabause | 3 — Limited |
| PSP | `psp/` | `.iso`, `.cso` | PPSSPP | 3 — Limited |
| Nintendo 3DS | `3ds/` | `.3ds`, `.cia` | Azahar | 3 — Experimental |

> **Neo Geo:** Place `neogeo.zip` in **both** `Config/BIOS/` and `Config/ROMs/neogeo/`.

> **N64 tip:** N64 has ~60–70% compatibility on the A15 without JIT. Complex titles (Conker, Rogue Squadron) may have issues. Use per-game overrides for problem titles — see [Per-Core Overrides](#15-per-core-override-reference).

### Directory Structure Example

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

## 4. Controller Setup

The Apple TV supports up to four simultaneous Bluetooth controllers. The Siri Remote navigates menus only.

### Pairing

1. **Apple TV:** Settings → Remotes and Devices → Bluetooth.
2. Put the controller into pairing mode (see table).
3. Select the controller under "Other Devices."
4. Launch RetroArch — the controller is recognized automatically.

### Compatibility

| Controller | Pairing Method | Status |
|-----------|---------------|--------|
| PS5 DualSense / Edge | PS + Create buttons | **Recommended** — full support |
| Xbox Series X/S Wireless | Xbox + Connect button | **Recommended** — full support |
| PS4 DualShock 4 | PS + Share buttons | Excellent — most stable |
| 8BitDo Pro 2 / Ultimate | Mode D/A + Pair button | Excellent — multi-platform |
| SteelSeries Nimbus+ | Bluetooth button | Good — MFi certified |
| Nintendo Switch Pro | Sync button | **Avoid** — B button exits app ([#18286](https://github.com/libretro/RetroArch/issues/18286)) |

### Known Controller Issues

- **Ghost inputs with multiple controllers** ([#18447](https://github.com/libretro/RetroArch/issues/18447)): Inputs from controllers 2+ may bleed into controller 1. Test multi-controller setups before relying on them.
- **DualSense latency advantage:** BT 5.1 vs DualShock 4's BT 4.0 gives noticeably lower latency. Also supports configurable rumble/haptics.

---

## 5. Installing RetroArch

No sideloading, developer accounts, or jailbreaking required.

1. **App Store:** Search for "RetroArch" (Daniel De Matteis) → tap Get.
2. **Launch:** A Welcome popup displays two URLs (local IP and Bonjour hostname). Write them down.
3. **Online Updater** (Main Menu → Online Updater):
   - Update Assets (icons, fonts, overlays)
   - Update Core Info Files
   - Update Databases
   - Update Slang Shaders

> **Tip:** RetroArch v1.20.0 added Bluetooth keyboard support on tvOS for text input, search, and core option editing.

---

## 6. Transferring ROM Files

tvOS has no Files app. All transfers use RetroArch's built-in network servers.

### Method A: Web Interface

1. Ensure computer and Apple TV are on the same network.
2. Launch RetroArch (must remain running).
3. Browse to the URL from the welcome popup.
4. Navigate into the `Config` folder.
5. Create a `ROMs` folder with subfolders per system (see [ROM Folder Reference](#3-rom-folder-reference)).
6. Drag and drop ROMs into the appropriate subfolder.

### Method B: WebDAV (Bulk Transfers)

Available in RetroArch v1.20.0+. Port 8080.

- **macOS:** Finder → Go → Connect to Server (⌘K) → `http://appletv.local:8080`
- **Windows:** File Explorer → right-click This PC → Map Network Drive → `http://appletv.local:8080`

Copy files into `Config/ROMs/` subfolders.

> **Tip:** Use wired Ethernet (128 GB model) for faster, more reliable transfers.

---

## 7. BIOS Files

Most 8/16-bit cores have built-in BIOS support. Several 32-bit+ systems require external BIOS files.

| System | Required BIOS File(s) | Required? |
|--------|----------------------|-----------|
| PlayStation 1 | `scph5501.bin` (NA) or `scph1001.bin` | Yes |
| Sega CD / Mega CD | `bios_CD_U.bin`, `bios_CD_E.bin`, `bios_CD_J.bin` | Yes |
| Sega Saturn | `sega_101.bin`, `mpr-17933.bin` | Yes |
| TurboGrafx-CD | `syscard3.pce` | Yes |
| Neo Geo | `neogeo.zip` | Yes |
| Nintendo DS | `bios7.bin`, `bios9.bin`, `firmware.bin` | Yes |
| Game Boy Advance | `gba_bios.bin` | Optional |
| NES / SNES / Genesis / GB / GBC | None | Not required |

### Placement

1. Open the web interface → navigate to `Config/BIOS/` (create if needed).
2. Upload BIOS files.
3. **Neo Geo:** Place `neogeo.zip` in both `Config/BIOS/` and `Config/ROMs/neogeo/`.

> **BIOS filenames are case-sensitive.** Use exact names (e.g., `scph5501.bin`, not `SCPH5501.BIN`).

---

## 8. Scanning and Loading Games

**Manual Scan** is the recommended method.

1. Main Menu → Import Content → Manual Scan.
2. **Content Directory:** Browse to your ROMs folder, select a system subfolder.
3. **System Name:** Choose the matching platform.
4. **Default Core:** Select the emulation core.
5. **Start Scan.** Repeat for every system.
6. System playlists appear under Main Menu → Playlists.

---

## 9. Storage Persistence (Critical)

> ⚠️ **This is the most important section.** Understanding tvOS storage behavior prevents data loss.

### The Problem

tvOS guarantees only **500 KB** of persistent storage per app. Everything else — ROMs, saves, BIOS, shaders — is purgeable cache. tvOS silently deletes cached data when storage runs low.

### Automatic Config Backup

Since v1.19.1, RetroArch stores `retroarch.cfg` in NSUserDefaults (the 500 KB persistent area). Shader assets are re-extracted automatically when detected missing.

### The Solution: Config Folder + iCloud Sync

1. Create `ROMs/` and `BIOS/` folders inside `Config/`.
2. Upload ROM and BIOS files into appropriate subfolders.
3. Set **File Browser** to `Config/ROMs/` (Settings → Directory).
4. Set **System/BIOS** to `Config/BIOS/` (Settings → Directory).
5. Enable **iCloud Cloud Sync:** Settings → Saving → Cloud Sync → Enable → Backend: iCloud.
6. If uploading the companion `retroarch.cfg`: quit and relaunch to load it. **Do NOT** use "Save Current Configuration" first — it overwrites the uploaded file.

> **ROM files are never synced.** Always maintain backups on your computer.

---

## 10. User Interface Configuration

RetroArch defaults to Ozone. For a TV-friendly console-style crossbar interface:

1. Settings → Drivers → Menu → change to **XMB**.
2. Quit and relaunch RetroArch.

---

## 11. Hotkey Configuration

The PS/Xbox home button opens tvOS Control Center, not RetroArch's menu. A controller combo is required.

### Menu Toggle

1. Settings → Input → Hotkeys.
2. Set **Menu Toggle Controller Combo** to **L3 + R3** (click both thumbsticks).
3. Optionally set **Enable Hotkeys** button (e.g., Select/Share) as a modifier.

### Recommended Bindings

With Select as the Enable Hotkeys modifier:

| Action | Combo | Description |
|--------|-------|-------------|
| Open Quick Menu | L3 + R3 | Access shaders, saves, settings mid-game |
| Save State | Select + R1 | Save progress |
| Load State | Select + L1 | Restore saved state |
| Fast Forward | Select + R2 (hold) | Speed up gameplay |
| Rewind | Select + L2 (hold) | Rewind frame by frame |
| State Slot + | Select + D-Pad Right | Next save slot |
| State Slot – | Select + D-Pad Left | Previous save slot |
| Close Content | Select + Start | Exit game, return to menu |

> **Without Close Content configured, there is no way to exit a running game.** You would have to force-quit the entire app.

### Netplay

RetroArch supports peer-to-peer Netplay on Apple TV with up to 16 players and spectators. Wired Ethernet recommended. Only cores with serialization (save state) support are compatible.

---

## 12. CRT Shaders

CRT shaders simulate scanlines, phosphor glow, and curvature for authentic visuals.

### Applying a Shader

1. Launch a game → Quick Menu (L3 + R3) → Shaders → Video Shaders: **ON**.
2. Load Preset → `shaders_slang` → `crt` → choose a preset.
3. Save Preset → **Save Core Preset** (applies to all games on that core).

### Recommended Shaders

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

> **Avoid on Apple TV:** CRT-Royale, CRT-Geom-Deluxe, Guest-Dr-Venom, Guest-Advanced, and all Mega Bezel shaders — too GPU-intensive.

> **Shader troubleshooting:** If `shaders_slang/crt/` appears empty, try Online Updater → Update Slang Shaders. If that fails ([#16083](https://github.com/libretro/RetroArch/issues/16083)), manually upload presets via WebDAV to `Config/shaders_slang/crt/`.

---

## 13. Cross-Device iCloud Sync

Save on one Apple device, continue on another.

### Setup

1. Install RetroArch on each device with the same Apple ID.
2. Settings → Saving → Cloud Sync → Enable → **ON**.
3. Backend: **iCloud**.
4. Ensure "Sort Saves into Folders by Core Name" is consistent across devices.

### What Syncs

| Data | Syncs? | Notes |
|------|--------|-------|
| Save files (.srm) | Yes | In-game saves |
| Save states | Yes | Quick save/load |
| Configuration | Yes | Core settings, hotkeys, directories |
| Thumbnails | Optional | Consumes iCloud quota |
| BIOS files | Optional | Enable system file sync |
| ROM files | **No** | Keep backups on your computer |

### Known Sync Issues

- **tvOS 18.6 crash:** RetroArch may crash on launch with iCloud Sync enabled. Disable temporarily if affected.
- **tvOS↔macOS conflicts** ([#16727](https://github.com/libretro/RetroArch/issues/16727)): Close content and return to main menu before quitting.
- `retroarch.cfg` does **not** sync via Cloud Sync. Upload manually to each device.
- **iCloud Drive backend** (PR [#17814](https://github.com/libretro/RetroArch/pull/17814)): Available on macOS/iOS but **not** Apple TV. Continue using the CloudKit backend.

---

## 14. Performance Optimization

### Apple TV Output

1. tvOS Settings → Video and Audio → **4K SDR 60 Hz**. Avoid HDR for retro gaming.
2. Enable Match Content → **Match Frame Rate**. Leave Match Dynamic Range **OFF**.
3. Enable **Game Mode** on your TV.

### RetroArch Video

1. Drivers → Video: confirm **Metal**.
2. Video → Synchronization: **V-Sync ON**.
3. Video → Scaling → **Integer Scale ON**.
4. Aspect Ratio: **Core Provided**.
5. **Bilinear Filtering OFF.** Do **NOT** enable Threaded Video — it crashes on tvOS ([#14978](https://github.com/libretro/RetroArch/issues/14978)).

### Latency Reduction

1. Latency → Preemptive Frames → **ON**, set to **1** (already in companion config).
2. Latency → Automatic Frame Delay → **ON** (reclaims unused rendering time).
3. Input → Input Poll Type Behavior → **Late**.
4. Video → Synchronization → Max Swapchain Images → **2** (may have no effect on Metal — verify in menu).

> **N64:** Disable Automatic Frame Delay via per-core override — doesn't work with Mupen64Plus-Next ([#14201](https://github.com/libretro/RetroArch/issues/14201)).

### TV Color Output

Enable **YCbCr 4:4:4** or **RGB Full** in your TV's HDMI input settings. Without 4:4:4, color subsampling causes bleeding at pixel boundaries.

### Variable Refresh Rate

Apple TV does **not** support gaming VRR. Only QMS VRR for media is supported. Leave "Sync to Exact Content Framerate" **OFF**.

---

## 15. Per-Core Override Reference

Override files: `Config/config/<core_name>/<core_name>.cfg`
Create via GUI: Quick Menu → Overrides → Save Core Overrides.

| Core | Overrides |
|------|-----------|
| **Mesen** (NES) | `rewind_enable = "true"`, `run_ahead_frames = "1"` |
| **Snes9x** (SNES) | `rewind_enable = "true"`, `run_ahead_frames = "1"` (bump to 2 for SMW/Zelda if no artifacts) |
| **mGBA** (GBA) | `rewind_enable = "true"`, `run_ahead_frames = "1"`, `audio_latency = "32"` |
| **Genesis Plus GX** | `rewind_enable = "true"`, `run_ahead_frames = "1"` |
| **Beetle PCE** | `rewind_enable = "true"`, `run_ahead_frames = "1"` |
| **FinalBurn Neo** | `rewind_enable = "true"`, `run_ahead_frames = "1"` |
| **PCSX ReARMed** (PS1) | `run_ahead_frames = "2"`, `audio_latency = "48"`, `rewind_enable = "false"` |
| **Mupen64Plus-Next** (N64) | `run_ahead_frames = "1"`, `rewind_enable = "false"` ([#18300](https://github.com/libretro/RetroArch/issues/18300)), `video_frame_delay_auto = "false"` ([#14201](https://github.com/libretro/RetroArch/issues/14201)), `audio_latency = "64"` |
| **melonDS DS** (NDS) | `run_ahead_frames = "1"`, `rewind_enable = "false"` |
| **Yabause** (Saturn) | `preemptive_frames_enable = "false"`, `run_ahead_frames = "0"`, `rewind_enable = "false"`, `video_shader_enable = "false"` |
| **PPSSPP** (PSP) | `video_driver = "gl"` (Metal/Vulkan crash), `preemptive_frames_enable = "false"`, `run_ahead_frames = "0"`, `rewind_enable = "false"`, `video_shader_enable = "false"` |
| **Azahar** (3DS) | `preemptive_frames_enable = "false"`, `run_ahead_frames = "0"`, `rewind_enable = "false"`, `video_shader_enable = "false"` |

---

## 16. Apple TV 4K 4th Gen (2026) — Projected Impact

> **Disclaimer:** Based on rumoured specs as of March 2026. Apple has not announced this product. Projections assume the App Store JIT restriction persists.

### Hardware Comparison

| Spec | 3rd Gen (2022) | 4th Gen (rumoured) |
|------|---------------|-------------------|
| SoC | A15 Bionic (5 nm, 5-core CPU) | A17 Pro (3 nm, 6-core CPU) |
| GPU | 4-core, no hardware RT | 6-core, hardware ray tracing |
| RAM | 4 GB | 8 GB |
| Wireless | Wi-Fi 6, BT 5.0 | Wi-Fi 7, BT 6 (Apple N1) |
| Process | 5 nm (TSMC N5P) | 3 nm (TSMC N3B) |

### Projected Tier Changes

- **PSP:** Promoted from Tier 3 → Tier 2. ~30% CPU gain makes most 2D/RPG titles playable. 3D-heavy titles remain below full speed.
- **3DS:** Promoted from Experimental → Tier 3. Lighter titles near full speed; demanding 3D still drops frames.
- **N64:** ~75–85% compatibility (up from 60–70%). Complex titles still limited without JIT.
- **PS1:** Full speed on all titles. Run-ahead 2 becomes safe globally.
- **Dreamcast/GC/Wii/PS2:** Still blocked by JIT restriction regardless of hardware.

### What Does Not Change

| Constraint | Reason |
|-----------|--------|
| App Store JIT restriction | Apple policy, not hardware |
| 500 KB persistent storage | tvOS platform limit |
| No VRR for gaming | QMS only (media frame-rate switching) |
| Siri Remote not a gamepad | Hardware limitation |
| Switch Pro B-button bug | Software bug [#18286](https://github.com/libretro/RetroArch/issues/18286) |
| Threaded video crash | RetroArch tvOS bug [#14978](https://github.com/libretro/RetroArch/issues/14978) |

---

## 17. Known Issues

| Issue | Status | Workaround |
|-------|--------|------------|
| PPSSPP + Vulkan/Metal crash ([#18050](https://github.com/libretro/RetroArch/issues/18050)) | Open | `video_driver = "gl"` per-core override |
| Switch Pro B-button exits app ([#18286](https://github.com/libretro/RetroArch/issues/18286)) | Open | Avoid Switch Pro Controller |
| Ghost inputs multi-controller ([#18447](https://github.com/libretro/RetroArch/issues/18447)) | Open | Use single controller or test carefully |
| iCloud Sync crash tvOS 18.6 | Reports | Disable Cloud Sync temporarily |
| Mupen64 rewind freeze ([#18300](https://github.com/libretro/RetroArch/issues/18300)) | Open | Disable rewind per-core |
| Mupen64 auto frame delay ([#14201](https://github.com/libretro/RetroArch/issues/14201)) | Open | Disable auto frame delay per-core |
| N64 rendering glitches ([#16598](https://github.com/libretro/RetroArch/issues/16598)) | Open | Per-game overrides |
| Threaded video crash ([#14978](https://github.com/libretro/RetroArch/issues/14978)) | Persists | Keep `video_threaded` omitted |
| Empty shader dir after update ([#16083](https://github.com/libretro/RetroArch/issues/16083)) | Partial | Re-run Online Updater; manual WebDAV upload |
| Cloud Sync tvOS↔macOS conflict ([#16727](https://github.com/libretro/RetroArch/issues/16727)) | Open | Close content before quitting |

---

## 18. Quick Reference: Setup Checklist

- [ ] RetroArch installed from tvOS App Store
- [ ] Online Updater run: Assets, Core Info, Databases, Slang Shaders
- [ ] `retroarch.cfg` uploaded via web/WebDAV and loaded on relaunch (**do NOT** Save Current Config first)
- [ ] Bluetooth controller paired (DualSense or Xbox Series recommended)
- [ ] Menu Toggle verified (L3 + R3 opens Quick Menu in-game)
- [ ] Hotkeys configured (Settings → Input → Hotkeys)
- [ ] ROMs placed in `Config/ROMs/<system>/` (see [folder reference](#3-rom-folder-reference))
- [ ] BIOS files placed in `Config/BIOS/` (case-sensitive filenames)
- [ ] Manual Scan completed per system
- [ ] CRT shader applied per-core (Quick Menu → Shaders → Save Core Preset)
- [ ] iCloud Cloud Sync enabled
- [ ] Apple TV output: 4K SDR 60 Hz
- [ ] TV Game Mode enabled
- [ ] `aspect_ratio_index` verified as "Core Provided"
- [ ] Quit and relaunch to trigger initial iCloud sync
- [ ] Automatic Frame Delay enabled
- [ ] Input Poll Type Behavior set to Late
- [ ] Max Swapchain Images set to 2 (verify on Metal)
- [ ] Chroma 4:4:4 / RGB Full enabled on TV
- [ ] Match Dynamic Range set to OFF
- [ ] Per-core overrides saved for N64 (disable auto frame delay + rewind)
- [ ] Per-core overrides saved for PSP (switch to GL, disable latency features)

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
