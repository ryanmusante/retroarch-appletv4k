2026-04-12  Ryan Musante

- v2.45: switch the package from a global low-latency preset to a conservative baseline; sync `README.md`, `CHANGELOG.md`, and `retroarch.cfg`.
- v2.45: disable global Run-Ahead (`run_ahead_enabled = "false"`) while keeping `run_ahead_frames = "1"` as a ready preset for later per-core enablement.
- v2.45: disable Automatic Frame Delay globally (`video_frame_delay_auto = "false"`) and document it as an optional per-core tuning step rather than a baseline default.
- v2.45: raise `video_max_swapchain_images` from `2` to `3` and `audio_latency` from `32` to `64` for a safer fixed-refresh tvOS baseline.

2026-04-12  Ryan Musante

- v2.44: sync `README.md`, `CHANGELOG.md`, and `retroarch.cfg` after the low-latency profile update.
- v2.44: switch the global latency method from Preemptive Frames to classic Run-Ahead (`run_ahead_enabled = "true"`, `run_ahead_frames = "1"`, `preemptive_frames_enable = "false"`).
- v2.44: reintroduce explicit directory assignments in `retroarch.cfg` (`system_directory = "config/BIOS"`, `rgui_browser_directory = "config/ROMs"`) and update README text to match.
- v2.44: add explicit `aspect_ratio_index = "22"` and `video_sync_to_exact_content_framerate = "false"` for deterministic video behavior.
- v2.44: lower `audio_resampler_quality` from `3` to `2` and `fastforward_ratio` from `5.0` to `3.0`; sync README tables to those values.

retroarch-appletv4k changelog

2026-04-11  Ryan Musante
- v2.43: sync README and CHANGELOG with the final companion `retroarch-configs` archive layout.
- v2.43: document that the companion ZIP ships `.cfg` / `.opt` files flat under `config/`, then must be moved manually into `config/<core_name>/` on the Apple TV for auto-loading.
- v2.43: no `retroarch.cfg` runtime keys changed in this release; documentation-only sync.

2026-04-11  Ryan Musante
- v2.42: remove stale Cloud Sync README section, set cloud_sync_enable false, and narrow network-interface wording


- v2.41: restore trimmed README summary, filesystem notes, and saving table
  rows without reintroducing Cloud Sync guidance.
- v2.40: clear the inactive `cloud_sync_driver` value and remove the remaining
  Cloud Sync / iCloud comments from `retroarch.cfg`.
- v2.39: document Mupen64Plus-Next as using Angrylion software RDP
  plus `cxd4` RSP from the companion retroarch-configs pack.
- v2.39: align Nintendo 64 README notes with the shipped Mupen64Plus-Next
  override profile.
- v2.38: disable all `cloud_sync_sync_*` settings in `retroarch.cfg` for
  local-only use and remove Cloud Sync guidance from `README.md`.
- v2.37: reduce `content_favorites_size` from `100` to `10` and
  `content_history_size` from `50` to `10` in `retroarch.cfg`; sync README
  menu table to `Favorites / History Size = 10 / 10`.

2026-04-11  Ryan Musante

- v2.35: sync README, changelog, and `retroarch.cfg` after removing path-based directory assignments from the shipped config.
- v2.35: remove `rgui_browser_directory`, `system_directory`, `savefile_directory`, and `savestate_directory`; README now documents those paths as manual UI settings.
- v2.35: drop the remaining per-core path note from `retroarch.cfg`; README is now the sole layout reference.
- v2.35: README filesystem layout now shows per-core override directories directly under `config/` (for example `config/Mesen/`), matching the tested auto-load layout and companion `retroarch-configs` README.

2026-04-09  Ryan Musante

- v2.34: CRIT — rename savefile_file_compression → save_file_compression
  (configuration.c:1842). The older key did not exist and was silently
  ignored — SRAM .srm files were NOT being compressed prior to this release
  despite the stated intent. Existing saves re-compress on next write.
- v2.34: CRIT — rename cloud_sync_backend → cloud_sync_driver
  (SETTING_ARRAY, configuration.c:1617). The older key did not exist and
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
  2-frame preemptive → Tier 2 overrides disable entirely). Prior comment
  said "0 for Tier 2" but omitted that Tier 1 runs 2-frame, not 1.
- v2.34: README Additional settings table — drop Recording row (no such
  key) and Left Thumbnails row (now default); version badge 2.33 → 2.34.

2026-04-06  Ryan Musante

- v2.33: retroarch.cfg L55-56 overscale comment lists all 5 cores (was 3).
- v2.33: CHANGELOG v2.32 entry "4 invalid keys" → "3" (matches enumeration).
- v2.32: rename 3 invalid keys (audio_output_rate → audio_out_rate;
  65 → 68 active keys.
- v2.31: video_max_swapchain_images comment — note 3 on 120 Hz panels.

2026-04-05  Ryan Musante

- v2.30: add compression and runahead keys; remove 5 tvOS defaults; net +4.
- v2.29: sync README §7, §11 video_threaded with retroarch.cfg.
- v2.21–2.28: add savestate_auto_index, block_sram_overwrite; remove 8
  tvOS defaults (72 → 64); consolidate README §7–§12; tvOS 26;

2026-04-04  Ryan Musante

- v2.20: move per-core overrides to retroarch-configs repo.

2026-04-03  Ryan Musante

- v2.19: add video_threaded, audio_output_rate, savestate_max_keep
  (68 → 72); remove config/ directory.

2026-04-02  Ryan Musante

- v2.17–2.18: add per-core override configs for 9 cores;
  add 23 keys (45 → 68); correct video_refresh_rate to 59.940060.

2026-04-01  Ryan Musante

- v2.14–2.16: fact-check corrections; add MIT LICENSE;

2026-03-31  Ryan Musante

- v2.11–2.13: add 13 keys (28 → 41);
  restructure README 14 → 13 sections; add badges.

2026-03-29  Ryan Musante

- v2.0–2.10: convert PDF to GitHub markdown; retarget to 64 GB Wi-Fi;
  iterative cfg tuning (25 → 28 keys).

2026-03-28  Ryan Musante

- v1.0: initial drop-in configuration (25 active keys).
