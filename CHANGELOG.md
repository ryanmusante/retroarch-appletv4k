2026-04-15  Ryan Musante

- v2.80: SYNC — no `retroarch.cfg` changes. Companion `retroarch-configs` bumped to v1.42: 4 explicit `.opt` pins added for drift-guard consistency (Mupen `angrylion-multithread`, PCSX `neon_enhancement_enable`, mGBA `audio_low_pass_range`, Genesis `render`); all match prior upstream defaults, no behavior change. `README.md` §8 CRT Shaders + §11 Setup Checklist: companion version refs `v1.41+` → `v1.42+`. Version badge 2.79 → 2.80.

2026-04-15  Ryan Musante

- v2.79: `retroarch.cfg` — remove `video_shader = "shaders_slang/crt/crt-easymode.slangp"` and its comment; shader assignments are now per-core in companion `retroarch-configs` v1.41+. `video_shader_enable = "true"` retained (pipeline gate for per-core shaders). 2 comments updated: `video_shader_enable` comment reworded for per-core context; `run_ahead_frames` comment clarified as "baseline". Key count 70 → 69.
- v2.79: `README.md` — §7 Video settings: Video Shaders row Notes updated for per-core shaders. §8 CRT Shaders: "Applying a shader" rewritten (no global default; per-core via `retroarch-configs` v1.41+); Recommended presets table: GBA removed from crt-easymode row (now uses lcd-grid-v2). §11 Setup Checklist: shader verify step updated for per-core (mGBA lcd-grid-v2 noted). Version badge 2.78 → 2.79. Companion `retroarch-configs` bumped to v1.41.

2026-04-13  Ryan Musante

- v2.78: `README.md` §6 Controllers — severity callout consistency pass. Added ⚠️ emoji to the "Ghost inputs from controllers 2+" Warning callout (L248) to match the other three severity callouts that already carried it: §3 Critical (L111), §4 Security (L151), §6 Warning "Close Content" (L273). Plain `> **Note:**` / `> **Saves and savestates:**` / `> **Integer Scaling Conflict:**` informational callouts intentionally remain emoji-free — only Critical/Security/Warning severity levels get ⚠️. No content or semantic changes. README line count unchanged. Companion `retroarch-configs` stays at v1.39.

2026-04-13  Ryan Musante

- v2.77: `retroarch.cfg` — strip section-header label prefixes from 7 comment lines. Removed: `# VIDEO:` (L16), `# AUDIO:` (L44), `# LATENCY:` (L56), `# INPUT:` (L74), `# PATHS:` (L80), `# CLOUD SYNC:` (L85), `# SAVES:` (L93). Comment bodies retained verbatim; only the `LABEL: ` prefix removed in each case. Key count unchanged (70). Comment count unchanged. CHANGELOG trimmed to the last 5 minor versions; older history removed. README version badge bumped 2.76 → 2.77. Companion `retroarch-configs` bumped to v1.39 (CHANGELOG trim + sync).
- v2.77: `README.md` §7 Video settings table — 8 rows brought to `(key = "value")` annotation style, matching the v2.73 pass that already normalized §7 Additional. Annotated: Video driver (`video_driver`), Integer Scale (`video_scale_integer`), Bilinear Filtering (`video_smooth`), Metal Argument Buffers (`video_use_metal_arg_buffers`), GPU Screenshot (`video_gpu_screenshot`), Crop Overscan (`video_crop_overscan`), Max Swapchain Images (`video_max_swapchain_images`), Refresh Rate (`video_refresh_rate`). Integer Overscale row left without a key (per-core concept; no global key exists).
- v2.77: `README.md` §7 Latency reduction table — Static Frame Delay row brought to `(key = "value")` style: `0` → `0 (\`video_frame_delay = "0"\`)`. Last row in the table that still lacked the annotation.
- v2.77: `README.md` §7 Additional settings table — add 3 missing rows for `retroarch.cfg` keys that were never documented despite the v2.70 "all 70 keys covered" claim. (1) Menu | Menu Driver | xmb (`menu_driver = "xmb"`) — XMB front-end, restart required. (2) Saving | Save Config on Exit | OFF (`config_save_on_exit = "false"`) — prevents accidental `retroarch.cfg` overwrite; aligns with §2 step 4 warning. (3) Video | Aspect Ratio | Core Provided (`aspect_ratio_index = "22"`) — promoted from §11 checklist mention to dedicated table row with index citation. All 70 `retroarch.cfg` keys now actually covered in README tables or §8.

2026-04-13  Ryan Musante

- v2.76: Companion `retroarch-configs` bumped to v1.38: `FinalBurn Neo.opt` added (placeholder, future use); `Beetle PCE Fast.opt` gains `pce_fast_cdspeed = "2"` (halves TurboGrafx-CD load times; default 1x).
