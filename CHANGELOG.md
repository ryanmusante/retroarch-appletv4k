2026-04-12  Ryan Musante

- v2.65: `retroarch.cfg` — remove `savestate_auto_save` and `savestate_auto_load` (both were set to `"true"`; upstream defaults are `false`). Rationale: the auto-save-on-close / auto-load-on-reopen pair was originally added as a tvOS volatile-cache eviction safety net, but it conflicts with manual save-slot workflow — auto-load silently overwrites the in-memory state on content reopen, defeating `savestate_max_keep = "5"` slot rotation. `block_sram_overwrite = "true"` already covers the SRAM side of the eviction concern. `content_runtime_log` / `content_runtime_log_aggregate` were never set (upstream default `false` retained). `savestate_auto_index = "true"` retained — different key, drives `savestate_max_keep` slot numbering.
- v2.65: `retroarch.cfg` — orphaned comment on L96 ("auto-save state on content close, auto-load on reopen; safety net tvos eviction") removed; it described the two keys that were just deleted and no longer applied to the surviving `block_sram_overwrite` line below it.
- v2.65: Key count 72 → 70. Companion `retroarch-configs` bumped to v1.29.1 (SYNC entry only; no `.cfg` / `.opt` changes — audit confirmed neither file set either key).
- v2.65: `README.md` §10 Optimizations table — `Max Auto-Increment States` row value `10` → `5`. **Pre-existing drift from v2.60** (v2.60 bumped `savestate_max_keep` 10 → 5 in `retroarch.cfg` but did not update the README row). Caught during v2.65 audit pass, fixed now. Row rationale also expanded from "Limits storage usage" to cite the actual key and reasoning.
- v2.65: `README.md` §7 Hotkey Assignments — added warning block under the `Close Content` row noting that save state is not auto-captured on close as of v2.65. Users relying on the prior `savestate_auto_save` / `savestate_auto_load` pair (originally a tvOS volatile-cache eviction safety net) need to trigger Save State (Select + R1) manually before closing content. SRAM path unchanged (`block_sram_overwrite` + 60 s `autosave_interval`).

2026-04-12  Ryan Musante

 Two findings, both in `README.md`; no `retroarch.cfg` changes. Companion `retroarch-configs` stays at v1.29 (clean).
- v2.64: `README.md` §9 Mesen row — three-way contradiction resolved. Prior row claimed "No per-game overclock needed", but `config/Mesen.opt` comment reads "overclock per-game only; see README" and the configs README §8 lists `mesen_overclock_rate` alongside `snes9x_overclock` and `pcsx_rearmed_psxclock` as an intentional per-game knob. Row now reads: "Per-game: `mesen_overclock_rate` available for slowdown-prone titles (Battletoads, Recca) but not set globally — values that help one game can break another". Aligns with the shipped `.opt` comment and configs §8.
- v2.64: `README.md` — TOC/heading structural mismatch fixed. TOC listed §12 Files in This Repository, §13 Versioning, §14 License with numbered anchors (`#12-files-in-this-repository` etc.), but the corresponding H2 headings in the body were un-numbered (`## Files in This Repository`, `## Versioning`, `## License`). All three TOC anchors were broken — clicking them in the rendered README would have scrolled nowhere. Fix: numbered the three trailing headings to match the TOC convention already used by §1–§11. TOC↔heading parity restored (14 sections, 14 headings).
- v2.64: `README.md` — in-body TOC anchor references to those three sections updated in lockstep (`(#files-in-this-repository)` → `(#12-files-in-this-repository)`, same for versioning and license). All 14 TOC entries now resolve to their numbered H2 headings.

2026-04-12  Ryan Musante

- v2.63: DOC-SYNC — REV 5 follow-up. One residual §7 drift item the v2.62 pass missed; no `retroarch.cfg` changes.
- v2.63: `README.md` §7 Run-Ahead Mode row — carve out Beetle PCE Fast's 1-frame run-ahead exception. Prior row stated "critical for sustained Tier 1 Run-Ahead 2 on the passively-cooled A15" and listed Beetle PCE Fast in the same verified-stable cores group, implying all six cores use 2-frame run-ahead. Beetle PCE Fast has shipped `run_ahead_frames = "1"` since companion v1.26 (CDROM seek determinism). Row now correctly splits: "Mesen, Snes9x, mGBA, Genesis Plus GX, and FinalBurn Neo at `run_ahead_frames = 2`; Beetle PCE Fast at `run_ahead_frames = 1`". Mirrors the v1.28 configs README Frontend Override Keys row fix.

2026-04-12  Ryan Musante

- v2.62: DOC-SYNC — REV 4 follow-up pass. Three residual drift items the v2.61 pass missed, all in `README.md` (no `retroarch.cfg` changes). Companion `retroarch-configs` stays at v1.28 — it is clean against its own shipped files; all fixes are global-README-only.
- v2.62: `README.md` §7 Integer Overscale row — add FinalBurn Neo to the per-core overscale list (NES, SNES, Genesis, PCE, mGBA, **FBN**). FBN has shipped `video_scale_integer_scaling = "1"` since companion v1.26, but the §7 prose was never updated in lockstep with the `retroarch.cfg` L15 comment (which was fixed in v2.61).
- v2.62: `README.md` §9 FinalBurn Neo row — rewind-conflict note rephrased. Prior wording "Rewind conflicts with Run Ahead (#16374)" was orphaned context; `rewind_enable = "false"` is globally set, so the conflict is unreachable in-package. Now reads "Rewind remains globally disabled — do not re-enable per-game alongside Run Ahead". Mirrors the v1.28 fix already applied in the companion configs README.
- v2.62: `README.md` §9 PCSX-ReARMed row — phantom-key wording "Run Ahead / preemptive frames disabled per-core" corrected. Neither `run_ahead_enabled` nor `preemptive_frames_enable` is set in `PCSX-ReARMed.cfg`; both inherit the global `false` baseline. Row now states that explicitly.
- v2.62: `README.md` §9 Mupen64Plus-Next row — same phantom-key correction. Rewind, auto frame delay, and Run Ahead are all disabled globally, not per-core. Row rewritten to reflect the inheritance relationship and keep the #18300 and #14201 references as the "why".
- v2.62: `README.md` §10 Known Issue #3 workaround — "Disable rewind per-core for N64" → "`rewind_enable = "false"` set globally; do not re-enable per-game for N64". Same phantom-key class.
- v2.62: `README.md` §10 Known Issue #4 workaround — "Disable auto frame delay per-core" → "`video_frame_delay_auto = "false"` set globally; do not re-enable per-game for N64". Same phantom-key class. Retains the "refactored upstream in v1.20.0" historical note.
- v2.62: `README.md` §6 Controllers hotkey table Rewind row — "disabled globally, per-core only" reworded to "hotkey bound but rewind disabled globally — enable per-game only". Same phantom-per-core class; clarifies that the binding exists but no shipped `.cfg` re-enables the feature.

2026-04-12  Ryan Musante

- v2.61: DOC-SYNC — REV 3 audit pass reconciling `retroarch.cfg` inline comments and README prose with shipped values. No functional config changes.
- v2.61: `retroarch.cfg` — `audio_resampler_quality` comment corrected. Prior comment claimed "Kaiser resampler 3 baseline; Tier 2 overrides lower to 2" but the value on that line is `"2"` and Tier 2 cfgs do not override it. Comment now reads "Kaiser resampler 2 baseline; lower than upstream 3 for A15 headroom".
- v2.61: `retroarch.cfg` — fix misplaced comment above `audio_sync`. Prior comment "DRC grouped with video_refresh_rate" described `audio_rate_control_delta`, not `audio_sync`. Rewritten as "audio_sync paired with video_refresh_rate for pacing".
- v2.61: `retroarch.cfg` — integer-overscale per-core comment list gains FinalBurn Neo (Mesen/Snes9x/GPGX/Beetle PCE/mGBA/FBN). FBN started shipping `video_scale_integer_scaling="1"` in companion v1.26 but the global comment was never updated.
- v2.61: `README.md` §7 — Threaded Video row rewritten. Prior row claimed "per-core true for interpreter-bound cores (N64, PS1) via retroarch-configs overrides", which has been wrong since companion v1.26 flipped both Tier 2 cfgs to `"false"` as #14978 crash-defense anchors. New row correctly describes them as forensic anchors pinned to the global value.
- v2.61: `README.md` §10 Known Issue #6 — same inverted `video_threaded` claim corrected. Both `Mupen64Plus-Next.cfg` and `PCSX-ReARMed.cfg` pin `"false"`; effective value is `false` everywhere.
- v2.61: `README.md` §8 and §11 — bump stale `retroarch-configs` v1.23 cross-references to v1.27+. Companion repo is at v1.28 after this pass but the floor for the §8 CRT-shader override behavior is v1.27.
- v2.61: Companion `retroarch-configs` bumped to v1.28: Frontend Override Keys table rewritten to match shipped files (three phantom key rows removed, `video_threaded` direction corrected, `run_ahead_frames` values `0,2`→`1,2` to cover Beetle PCE Fast's 1-frame exception, `audio_latency` clarified as PS1-only override with N64 inheriting global); FBN rewind note rephrased (rewind is globally off so the "conflict" is unreachable in-package); File Structure section collapsed to a pointer at Manual Install as single source of truth for on-device layout.
- v2.60: AUDIT — REV 2 audit pass against libretro/RetroArch master `configuration.c` and `gfx/video_defines.h`. 5 REV 1 findings retracted after source verification (all were valid upstream-documented keys/values): `video_use_metal_arg_buffers`, `aspect_ratio_index=22` (= ASPECT_RATIO_CORE), `log_verbosity="false"` (default literal), `cloud_sync_driver=""`, Genesis Plus GX `lowpass_range="55"`. Corrected findings applied.
- v2.60: `retroarch.cfg` — `audio_rate_control_delta` 0.005 → 0.008. Upstream default 0.005 is tight for uncalibrated tvOS 59.94Hz drift with `vrr_runloop_enable=false`; 0.008 gives DRC headroom before audible crackle without impacting sync accuracy on calibrated setups.
- v2.60: `retroarch.cfg` — `autosave_interval` 30 → 60. 30s cadence contradicted the L131 "log_to_file wastes volatile cache space" rationale (same cache, same churn). SRAM still flushes on pause/close, so 60s loses at most 30s of extra progress vs. prior behavior.
- v2.60: `retroarch.cfg` — `savestate_max_keep` 10 → 5. 10 × N64 ~1 MB × N games was unbounded growth on the 64 GB model; 5 halves the worst-case ceiling.
- v2.60: `retroarch.cfg` — remove `menu_throttle_framerate="true"`. Valid key (verified in configuration.c) but effect is already covered by `video_max_swapchain_images=3` + `video_swap_interval=1` under the Metal driver. Key count 73 → 72.
- v2.60: `retroarch.cfg` — fix orphan comment "integer overscale per-core (Mesen/Snes9x/GPGX/Beetle PCE/mGBA)" which was sitting above the unrelated `video_gpu_screenshot` key. Moved above the `video_scale_integer` block where it belongs. Give `video_gpu_screenshot` its own accurate comment.
- v2.60: `retroarch.cfg` — remove orphan comment "left thumbnails default 0; key omitted" which referred to a non-shipped key and was misleading above `content_favorites_size`.
- v2.60: `retroarch.cfg` — rewrite `suspend_screensaver_enable` comment to document the tvOS limitation honestly: tvOS ignores `UIApplication.isIdleTimerDisabled` while the app is paused, so the Aerial screensaver can still fire regardless of this setting. Best-effort only.
- v2.60: Companion `retroarch-configs` bumped to v1.26: 5 dead GLideN64 keys removed from Mupen64Plus-Next.opt, `video_threaded=true` → `false` in Mupen64Plus-Next.cfg and PCSX-ReARMed.cfg (aligning with global #14978 defense), FinalBurn Neo gains `video_scale_integer_scaling`, Beetle PCE Fast run-ahead 2 → 1, mGBA interframe_blending mix_smart → mix, orphan comments removed from Snes9x/Mesen .cfg.

2026-04-12  Ryan Musante

- v2.59: STYLE — shorten every comment in `retroarch.cfg` without removing any. 43 comments tightened, 638 bytes saved. All 60 comments preserved, every line now under 80 chars. 60 → 60 comment lines, 73 → 73 key lines.

2026-04-12  Ryan Musante

- v2.58: README trim — drop Legal Notice blockquote in §1 (A1; duplicated inline elsewhere); drop §7 User interface XMB subsection (A3; cosmetic); trim §7 Refresh Rate notes to the calibration action (A4); drop empty-Notes rows V-Sync, Aspect Ratio, Input Poll Type Behavior (A5/A6/A7); consolidate 4 §7 Additional settings Security rows into 1 (A8); drop CRT shaders definition sentence in §8 intro (A9); drop Known Issues #8 (HDMI cable advice — not RetroArch-specific) and #9 (A15 thermal — duplicated elsewhere) (A11/A12); drop entire Appendix A 4th Gen Projections (A13; speculation about unreleased hardware); compress §5 Scanning games 7-step procedure to 1 line (A14). TOC renumbered: Files/Versioning/License entries 13/14/15 → 12/13/14.
- v2.58: 31924 → 29183 bytes (8.6% size reduction), zero loss of actionable content.

2026-04-12  Ryan Musante

- v2.57: `retroarch.cfg` — add explicit `savefile_directory = "config/saves"` and `savestate_directory = "config/states"`. Saves and states are now backup-accessible via WebDAV under the sandboxed root. With `sort_savefiles_enable` / `sort_savestates_enable` at their upstream defaults of true, both are auto-organized into per-core subfolders (e.g. `config/saves/Mesen/`). Reverts the v2.35 removal of these keys. Note: existing saves in RetroArch's platform-managed default path will not auto-migrate — copy them into `config/saves/<core>/` manually if needed.
- v2.57: README §4 Filesystem layout — restore `saves/` and `states/` entries to the diagram and trim the verbose "Saves and savestates" note from ~10 sentences down to 2, reflecting the new explicit-path behavior.
- v2.57: retroarch.cfg key count 71 → 73.

2026-04-12  Ryan Musante

- v2.56: README §4 Filesystem layout — remove aspirational `config/saves/` and `config/states/` directories from the diagram. Those paths were documented but never enforced by `retroarch.cfg` (no `savefile_directory` / `savestate_directory` keys set since v2.35 removed them). RetroArch upstream default `DEFAULT_SAVEFILES_IN_CONTENT_DIR = false` puts saves in the platform-managed default path, auto-sorted into per-core subfolders via `sort_savefiles_enable = true`. Add explanatory note documenting actual behavior, the tradeoff vs explicit `config/saves/` paths, and how to opt in to backup-accessible paths if desired.
- v2.56: no `retroarch.cfg` key changes — documentation-only correction aligning README with shipped config reality.

2026-04-12  Ryan Musante

- v2.55: `retroarch.cfg` — add `video_shader = "shaders_slang/crt/crt-easymode.slangp"` as the global default CRT preset. Every core inherits crt-easymode automatically; companion `retroarch-configs` v1.23 Tier 2 overrides replace it with `zfast_crt.slangp` to fit the A15 GPU budget under Angrylion software RDP (N64) and the PSX interpreter. `video_shader_enable` was already set to true; no longer inert.
- v2.55: README §8 "Applying a shader" — rewrite to reflect the new global default loading automatically, with Tier 2 override note. Setup checklist shader row updated from "applied manually per core" to automatic global + Tier 2 override verification.
- v2.55: companion `retroarch-configs` bumped to v1.23 (Tier 2 zfast_crt explicit overrides + core summary table refresh).
- v2.55: retroarch.cfg key count 70 → 71.

2026-04-12  Ryan Musante

- v2.54: PERF — `retroarch.cfg` add 4 explicit `false` values for keys whose upstream defaults are ON and hurt A15 tvOS performance: `netplay_public_announce` (network traffic), `menu_enable_widgets` (animated notification GPU compositing), `video_waitable_swapchains` (non-UWP-Windows default true; adds frame-latency pacing overhead on tvOS Metal). Also added `discord_allow = "false"` explicit (upstream default is already false) for audit symmetry with the security block.
- v2.54: STYLE — collapse multiline comment runs in `retroarch.cfg` to single-line comments per the same policy applied to `.cfg`/`.opt` files. 56 → 55 comment lines.
- v2.54: README §7 Additional settings table — add 4 new rows documenting the above explicit values and their rationale.
- v2.54: retroarch.cfg key count 66 → 70.

2026-04-12  Ryan Musante

- v2.53: PERF — `retroarch.cfg` add `run_ahead_secondary_instance = "false"` (RetroArch upstream default is `true`). Forces Single Instance Run-Ahead (save-state rewind within one core) instead of Second Instance (two parallel core instances). Same latency benefit (Run-Ahead 2 still active on all Tier 1 cores) at roughly half the CPU cost. Single Instance tested stable across the Tier 1 set; critical for sustained Run-Ahead 2 + CRT shader on the passively-cooled A15.
- v2.53: README §7 Latency reduction table — add "Run-Ahead Mode" row documenting Single Instance baseline, tradeoff vs Second Instance, and the per-core escape hatch (`run_ahead_secondary_instance = "true"` override) for any core that later exhibits audio crackle or serialization glitches.
- v2.53: retroarch.cfg key count 65 → 66.

2026-04-12  Ryan Musante

- v2.52: README §9 Tier 2 PCSX-ReARMed row — update `psxclock` guidance from "lower to 50 per-game for demanding titles" (which assumed the old broken global of 57) to "global is 100 native; per-game underclock to 75 or 50 for demanding 3D titles" matching the corrected `retroarch-configs` v1.20 baseline.
- v2.52: companion `retroarch-configs` bumped to v1.20 (Genesis Plus GX MAME YM2612, Mupen64Plus-Next CopyDepthToRDRAM Off, PCSX-ReARMed psxclock 100 + async GPU threading).
- v2.52: no `retroarch.cfg` runtime keys changed.

2026-04-12  Ryan Musante

- v2.51: CRIT — `retroarch.cfg` L55 delete `video_sync_to_exact_content_framerate = "false"`. There is no such RetroArch key — verified absent from `libretro/RetroArch/configuration.c` and `config.def.h` on master. The string "Sync to exact content framerate" is the menu *label* for the existing `vrr_runloop_enable` key, which is already correctly set to `false` on L57. The bogus key was silently ignored by RetroArch loaders since v2.44; no runtime behavior changes from this deletion.
- v2.51: README §7 Latency reduction table — correct the key reference for the "Sync to Exact Content Framerate" row from `video_sync_to_exact_content_framerate` to `vrr_runloop_enable`.
- v2.51: full audit of all 66 `retroarch.cfg` keys against upstream `configuration.c` SETTING_* macros — only the bogus key above flagged; the remaining 65 keys verified valid (`log_verbosity` is read via `config_get_bool` rather than registered through SETTING_*, but is a documented and accepted key).
- v2.51: companion `retroarch-configs` bumped to v1.19 (Mupen64Plus-Next Player 1 Rumble Pak default + full opt/cfg audit).

2026-04-12  Ryan Musante

- v2.50: README — add missing TOC entries for `Files in This Repository` (pre-existing gap, never linked), `Versioning` (added in v2.49 but never linked), and `License`.
- v2.50: no `retroarch.cfg` runtime keys changed; no other README content changed.

2026-04-12  Ryan Musante

- v2.49: README — close §9 numbering gap; renumber §10 (Supported Systems) → §9, §11 (Known Issues) → §10, §12 (Setup Checklist) → §11, and Appendix A TOC entry 13 → 12; update all internal anchor links (`#10-` → `#9-`, etc.) and bare §-references in the filesystem layout block, tier table footnote, and per-core override checklist row.
- v2.49: README — add Versioning section above License documenting the `vMAJOR.MINOR` scheme and kernel.org-style CHANGELOG convention.
- v2.49: no `retroarch.cfg` runtime keys changed; no other README content changed.

2026-04-12  Ryan Musante

- v2.48: CHANGELOG normalize — strip orphan `retroarch-appletv4k changelog` mid-file header; standardize all entries to kernel.org canonical (date header followed by blank line followed by bullets); unwrap continuation-wrapped bullets in v2.37–v2.41 entries; collapse stray multi-blank gaps.
- v2.48: no changes to `retroarch.cfg` or README content; CHANGELOG-only sync.

2026-04-12  Ryan Musante

- v2.47: bump companion `retroarch-configs` dependency to v1.15 — per-core `video_shader` assignments removed; CRT shaders now apply only when set manually via Quick Menu → Shaders → Save Core Preset.
- v2.47: README §8 — rewrite "Applying a shader" preassignment paragraph; setup checklist now flags manual shader application instead of auto-load verification.
- v2.47: no `retroarch.cfg` runtime keys changed in this release.

2026-04-12  Ryan Musante

- v2.46: sync `README.md`, `CHANGELOG.md`, and the companion `retroarch-configs` package after enabling Run-Ahead explicitly in all Tier 1 core overrides.
- v2.46: keep the global baseline conservative (`run_ahead_enabled = "false"`) while documenting that validated Tier 1 companion overrides turn Run-Ahead on per core.
- v2.46: document `PCSX-ReARMed.cfg` threaded video as a retained core-specific fallback; test `video_threaded = "false"` only if that core shows pacing issues.

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

2026-04-11  Ryan Musante

- v2.43: sync README and CHANGELOG with the final companion `retroarch-configs` archive layout.
- v2.43: document that the companion ZIP ships `.cfg` / `.opt` files flat under `config/`, then must be moved manually into `config/<core_name>/` on the Apple TV for auto-loading.
- v2.43: no `retroarch.cfg` runtime keys changed in this release; documentation-only sync.

2026-04-11  Ryan Musante

- v2.42: remove stale Cloud Sync README section, set cloud_sync_enable false, and narrow network-interface wording

- v2.41: restore trimmed README summary, filesystem notes, and saving table rows without reintroducing Cloud Sync guidance.
- v2.40: clear the inactive `cloud_sync_driver` value and remove the remaining Cloud Sync / iCloud comments from `retroarch.cfg`.
- v2.39: document Mupen64Plus-Next as using Angrylion software RDP plus `cxd4` RSP from the companion retroarch-configs pack.
- v2.39: align Nintendo 64 README notes with the shipped Mupen64Plus-Next override profile.
- v2.38: disable all `cloud_sync_sync_*` settings in `retroarch.cfg` for local-only use and remove Cloud Sync guidance from `README.md`.
- v2.37: reduce `content_favorites_size` from `100` to `10` and `content_history_size` from `50` to `10` in `retroarch.cfg`; sync README menu table to `Favorites / History Size = 10 / 10`.

2026-04-11  Ryan Musante

- v2.35: sync README, changelog, and `retroarch.cfg` after removing path-based directory assignments from the shipped config.
- v2.35: remove `rgui_browser_directory`, `system_directory`, `savefile_directory`, and `savestate_directory`; README now documents those paths as manual UI settings.
- v2.35: drop the remaining per-core path note from `retroarch.cfg`; README is now the sole layout reference.
- v2.35: README filesystem layout now shows per-core override directories directly under `config/` (for example `config/Mesen/`), matching the tested auto-load layout and companion `retroarch-configs` README.

2026-04-09  Ryan Musante

- v2.34: CRIT — rename savefile_file_compression → save_file_compression (configuration.c:1842). The older key did not exist and was silently ignored — SRAM .srm files were NOT being compressed prior to this release despite the stated intent. Existing saves re-compress on next write.
- v2.34: CRIT — rename cloud_sync_backend → cloud_sync_driver (SETTING_ARRAY, configuration.c:1617). The older key did not exist and not change, but the line is now functional rather than cosmetic.
- v2.34: CRIT — delete recording_enable = "false". There is no such RetroArch setting — recording is a runtime hotkey only, verified absent in configuration.c v1.22.x. Moved to the OMITTED header note.
- v2.34: delete menu_left_thumbnails = "0"; DEFAULT_MENU_LEFT_THUMBNAILS_DEFAULT is already 0 (config.def.h:1664), so the line was redundant under the file's own "purely redundant keys are omitted" policy.
- v2.34: rewrite audio_rate_control_delta comment — prior wording implied raising audio_max_timing_skew would help PAL 50 Hz content sync on a 60 Hz display. It will not: DRC only corrects sub-percent refresh drift.
- v2.34: rewrite preemptive_frames_enable header comment to document the effective Tier 1 layering (global 1-frame → Tier 1 overrides raise to 2-frame preemptive → Tier 2 overrides disable entirely). Prior comment said "0 for Tier 2" but omitted that Tier 1 runs 2-frame, not 1.
- v2.34: README Additional settings table — drop Recording row (no such key) and Left Thumbnails row (now default); version badge 2.33 → 2.34.

2026-04-06  Ryan Musante

- v2.33: retroarch.cfg L55-56 overscale comment lists all 5 cores (was 3).
- v2.33: CHANGELOG v2.32 entry "4 invalid keys" → "3" (matches enumeration).
- v2.32: rename 3 invalid keys (audio_output_rate → audio_out_rate; 65 → 68 active keys.
- v2.31: video_max_swapchain_images comment — note 3 on 120 Hz panels.

2026-04-05  Ryan Musante

- v2.30: add compression and runahead keys; remove 5 tvOS defaults; net +4.
- v2.29: sync README §7, §11 video_threaded with retroarch.cfg.
- v2.21–2.28: add savestate_auto_index, block_sram_overwrite; remove 8 tvOS defaults (72 → 64); consolidate README §7–§12; tvOS 26;

2026-04-04  Ryan Musante

- v2.20: move per-core overrides to retroarch-configs repo.

2026-04-03  Ryan Musante

- v2.19: add video_threaded, audio_output_rate, savestate_max_keep (68 → 72); remove config/ directory.

2026-04-02  Ryan Musante

- v2.17–2.18: add per-core override configs for 9 cores; add 23 keys (45 → 68); correct video_refresh_rate to 59.940060.

2026-04-01  Ryan Musante

- v2.14–2.16: fact-check corrections; add MIT LICENSE;

2026-03-31  Ryan Musante

- v2.11–2.13: add 13 keys (28 → 41); restructure README 14 → 13 sections; add badges.

2026-03-29  Ryan Musante

- v2.0–2.10: convert PDF to GitHub markdown; retarget to 64 GB Wi-Fi; iterative cfg tuning (25 → 28 keys).

2026-03-28  Ryan Musante

- v1.0: initial drop-in configuration (25 active keys).
