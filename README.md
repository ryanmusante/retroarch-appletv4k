# retroarch-appletv4k

![version](https://img.shields.io/badge/version-2.43-blue)
![device](https://img.shields.io/badge/device-Apple%20TV%204K%20(3rd%20gen)-black)
![license](https://img.shields.io/badge/license-MIT-green)

RetroArch setup for Apple TV 4K (3rd generation). This repo ships the global `retroarch.cfg` and documents the manual directory layout used on tvOS.

[changelog](CHANGELOG.md)

## Scope

- Metal video driver
- Slang shader pipeline
- XMB menu defaults
- low-latency global settings
- manual ROM, BIOS, and directory setup
- companion per-core overrides from `retroarch-configs`

## Files

| File | Purpose |
|---|---|
| `retroarch.cfg` | global RetroArch settings for Apple TV 4K |
| `README.md` | install and layout notes |

## Install

1. Install RetroArch on the Apple TV.
2. Open the built-in web interface or connect with WebDAV.
3. Upload `retroarch.cfg` to the RetroArch sandbox root.
4. Quit and relaunch RetroArch.
5. Set ROM, BIOS, save, and state directories manually in the UI.
6. Upload the companion `retroarch-configs` files under `config/<core>/`.

## Layout

All paths in this guide are relative to RetroArch's sandbox root.

```text
config/
  BIOS/
  ROMs/
  <core>/
    <core>.cfg
    <core>.opt
retroarch.cfg
```

Per-core overrides auto-load from `config/<core>/`. Quick Menu → Overrides → Save Core Overrides creates matching directories automatically.

## Supported systems

| System | Core | Tier |
|---|---|---:|
| NES | Mesen | 1 |
| SNES | Snes9x | 1 |
| GB / GBC / GBA | mGBA | 1 |
| Genesis / Mega Drive / Sega CD / Master System | Genesis Plus GX | 1 |
| PC Engine / TurboGrafx-16 | Beetle PCE Fast | 1 |
| Neo Geo / CPS1/2/3 | FinalBurn Neo | 1 |
| PlayStation 1 | PCSX-ReARMed | 2 |
| Nintendo 64 | Mupen64Plus-Next | 2 |

Tier 1 = full speed with default low-latency tuning. Tier 2 = heavier cores with relaxed latency.

Nintendo 64 uses the companion override profile with Angrylion RDP, cxd4 RSP, and Slang shaders.

## Required manual directories

| Purpose | Suggested path |
|---|---|
| BIOS | `config/BIOS/` |
| ROMs | `config/ROMs/<system>/` |
| save files | user-chosen path in the UI |
| save states | user-chosen path in the UI |

The shipped `retroarch.cfg` does not hardcode these directory paths.

## Global settings carried by `retroarch.cfg`

| Area | Setting |
|---|---|
| video | `video_driver = "metal"`, integer scale on, shaders enabled |
| menu | XMB, throttled menu framerate, small favorites/history lists |
| latency | preemptive frames globally, per-core overrides for heavy cores |
| saves | compression on, autosave enabled, capped auto-increment states |
| security | command and remote-control interfaces disabled |
| cloud sync | disabled |

Favorites / History Size is set to **10 / 10**.

## Controllers

Recommended: PS5 DualSense or Xbox Series controllers. Avoid Switch Pro on tvOS with RetroArch because the B button can exit the app.

Set a menu toggle combo such as **L3 + R3** under Settings → Input → Hotkeys.

## Notes

- `video_threaded` stays global `false`; heavy cores override it per-core.
- Cloud sync is disabled.
- Web interface and WebDAV remain the expected file-management path.

## Related

- `retroarch-configs` — per-core `.cfg` and `.opt` overrides

## License

MIT
