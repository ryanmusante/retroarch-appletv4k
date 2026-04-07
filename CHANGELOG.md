retroarch-appletv4k changelog

2026-04-06  Ryan Musante

- v2.33: melonDS DS Tier 2 → Tier 3 (sync with retroarch-configs v1.7).
- v2.33: drop PPSSPP from video_threaded per-core list (no override ships).
- v2.33: retroarch.cfg L55-56 overscale comment lists all 5 cores (was 3).
- v2.33: CHANGELOG v2.32 entry "4 invalid keys" → "3" (matches enumeration).
- v2.32: rename 3 invalid keys (audio_output_rate → audio_out_rate;
  cloud_sync_enable_thumbnails → cloud_sync_sync_thumbs;
  cloud_sync_enable_system → cloud_sync_sync_system); add explicit
  cloud_sync_sync_saves, cloud_sync_sync_configs, video_crop_overscan;
  65 → 68 active keys.
- v2.31: video_max_swapchain_images comment — note 3 on 120 Hz panels.

2026-04-05  Ryan Musante

- v2.30: add compression and runahead keys; remove 5 tvOS defaults; net +4.
- v2.29: sync README §7, §11 video_threaded with retroarch.cfg.
- v2.21–2.28: add savestate_auto_index, block_sram_overwrite; remove 8
  tvOS defaults (72 → 64); consolidate README §7–§12; tvOS 26;
  Azahar 2125.0.1; fix melonDS key prefix.

2026-04-04  Ryan Musante

- v2.20: move per-core overrides to retroarch-configs repo; add Azahar 3DS.

2026-04-03  Ryan Musante

- v2.19: add video_threaded, audio_output_rate, savestate_max_keep
  (68 → 72); remove config/ directory.

2026-04-02  Ryan Musante

- v2.17–2.18: add per-core override configs for 9 cores;
  add 23 keys (45 → 68); correct video_refresh_rate to 59.940060.

2026-04-01  Ryan Musante

- v2.14–2.16: fact-check corrections; add MIT LICENSE;
  add DRC, cloud sync, savestate keys (41 → 45).

2026-03-31  Ryan Musante

- v2.11–2.13: add 13 keys (28 → 41); replace Yabause with Beetle Saturn;
  restructure README 14 → 13 sections; add badges.

2026-03-29  Ryan Musante

- v2.0–2.10: convert PDF to GitHub markdown; retarget to 64 GB Wi-Fi;
  iterative cfg tuning (25 → 28 keys).

2026-03-28  Ryan Musante

- v1.0: initial drop-in configuration (25 active keys).
