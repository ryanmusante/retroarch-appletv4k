retroarch-appletv4k changelog

2026-04-06  Ryan Musante

- Tagged as v2.32
- retroarch.cfg: fix 4 invalid keys silently ignored by RetroArch parser:
  audio_output_rate → audio_out_rate;
  cloud_sync_enable_thumbnails → cloud_sync_sync_thumbs;
  cloud_sync_enable_system → cloud_sync_sync_system.
  Verified against libretro/RetroArch configuration.c (SETTING_UINT
  audio_out_rate) and configuration.h (bool cloud_sync_sync_thumbs,
  cloud_sync_sync_system).
- retroarch.cfg: add explicit cloud_sync_sync_saves="true" and
  cloud_sync_sync_configs="true" to pin documented intent.
- retroarch.cfg: add video_crop_overscan="true" to resolve README §7
  video table drift (was listed but absent from cfg).
- README: sync §7 additional settings table to corrected audio_out_rate key.
- Net key delta: 65 → 68 active keys (+3 added; 3 renamed in place).

2026-04-06  Ryan Musante

- Tagged as v2.31
- retroarch.cfg: L47 video_max_swapchain_images comment — note 3 on 120 Hz panels.
- Audit cycle closed: all v2.30 findings verified against repo; no key/value changes.

2026-04-05  Ryan Musante

- Tagged as v2.30
- retroarch.cfg: add compression and runahead keys (65→69), remove 5 tvOS defaults (69→65, net +4−5).
- retroarch.cfg: document preemptive frames / runahead relationship.
- README: update §7 additional settings and §12 checklist.
- Tagged as v2.29
- Sync README §7, §11 video_threaded with retroarch.cfg.
- retroarch.cfg: clarify per-core overrides vs Tier 3 guidance.
- Tagged as v2.21 – v2.28
- retroarch.cfg: add savestate_auto_index, block_sram_overwrite; remove 8 tvOS defaults (72→64).
- README: consolidate §7–§12; update tvOS 26, Azahar 2125.0.1; fix melonDS key prefix.

2026-04-04  Ryan Musante

- Tagged as v2.20
- README: move per-core overrides to retroarch-configs repo; add Azahar 3DS.

2026-04-03  Ryan Musante

- Tagged as v2.19
- retroarch.cfg: add video_threaded, audio_output_rate, savestate_max_keep (68→72).
- Remove config/ directory (moved to retroarch-configs).

2026-04-02  Ryan Musante

- Tagged as v2.17, v2.18
- Add per-core override configs for 9 cores.
- retroarch.cfg: add 23 keys (45→68); correct video_refresh_rate to 59.940060.

2026-04-01  Ryan Musante

- Tagged as v2.14 – v2.16
- Fact-check corrections; add MIT LICENSE.
- retroarch.cfg: add DRC, cloud sync, savestate keys (41→45).

2026-03-31  Ryan Musante

- Tagged as v2.11 – v2.13
- retroarch.cfg: add 13 keys (28→41); replace Yabause with Beetle Saturn.
- README: restructure 14→13 sections; add badges.

2026-03-29 – 2026-03-30  Ryan Musante

- Tagged as v2.0 – v2.10
- Convert PDF to GitHub markdown; retarget to 64 GB Wi-Fi model.
- retroarch.cfg: iterative tuning (25→28 keys).

2026-03-28  Ryan Musante

- Tagged as v1.0
- Initial drop-in configuration (25 active keys).
