retroarch-appletv4k changelog

2026-04-11  Ryan Musante

- v2.35: sync README, changelog, and `retroarch.cfg` after removing path-based directory assignments from the shipped config.
- v2.35: remove `rgui_browser_directory`, `system_directory`, `savefile_directory`, and `savestate_directory`; README now documents those paths as manual UI settings.
- v2.35: drop the remaining per-core path note from `retroarch.cfg`; README is now the sole layout reference.
- v2.35: README filesystem layout now shows per-core override directories directly under `Config/` (for example `Config/Mesen/`), matching the tested auto-load layout and companion `retroarch-configs` README.

2026-04-09  Ryan Musante

- v2.34: CRIT — rename savefile_file_compression → save_file_compression
  (configuration.c:1842). The older key did not exist and was silently
  ignored — SRAM .srm files were NOT being compressed prior to this release
  despite the stated intent. Existing saves re-compress on next write.
- v2.34: CRIT — rename cloud_sync_backend → cloud_sync_driver
  (SETTING_ARRAY, configuration.c:1617). The older key did not exist and
  was silently ignored; iCloud was the only tvOS backend so behavior did
  not change, but the line is now functional rather than cosmetic.
- v2.34: CRIT — delete recording_enable = "false". There is no such
  RetroArch setting — recording is a runtime hotkey only, verified absent
  in configuration.c v1.22.x. Moved to the OMITTED header note.
- v2.34: delete menu_left_thumbnails = "0"; DEFAULT_MENU_LEFT_THUMBNAILS_DEFAULT
  is already 0 (config.def.h:1664), so the line was redundant under the
  file's own "purely redundant keys are omitted" policy.
- v2.34: rewrite audio_rate_control_delta comment — prior wording implied
  raising audio_max_timing_skew would help PAL 50 Hz content sync on a
  60 Hz display. It will not: DRC only corrects sub-percent refresh drift.
- v2.34: rewrite preemptive_frames_enable header comment to document the
  effective Tier 1 layering (global 1-frame → Tier 1 overrides raise to
  2-frame preemptive → Tier 2-3 overrides disable entirely). Prior comment
  said "0 for Tier 2-3" but omitted that Tier 1 runs 2-frame, not 1.
- v2.34: README Additional settings table — drop Recording row (no such
  key) and Left Thumbnails row (now default); version badge 2.33 → 2.34.

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
