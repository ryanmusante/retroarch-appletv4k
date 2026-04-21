2026-04-20  Ryan Musante

- v3.4: SYNC — doc-alignment release; companion ships §4 drift fix.
  - `retroarch.cfg`: header version stamps bumped to v3.4; all
    65 key values byte-identical to v3.3.
  - README: landing paragraph + badge bumped 3.3 → 3.4.
  - Version badge 3.3 → 3.4. Companion bumped to v3.4
    (configs README §4 `run_ahead_frames` row: "All 7 Tier 1
    cores" → "All 6 Tier 1 cores" [stale count; 6 cores ship
    `run_ahead_frames = "2"`, matching §1 Tier 1 rollup]; all
    8 `.cfg` header "paired with" stamps bumped; no .cfg/.opt
    content changes).

2026-04-19  Ryan Musante

- v3.3: companion v3.3 Tier 2 stutter-mitigation alignment.
  - `retroarch.cfg`: `fastforward_ratio` "0.0" → "5.0" (uncapped FF
    drove audio underrun on Tier 2 interpreter stack; 5× cap
    transparent to Tier 2, preserves Tier 1 headroom; revert to
    "4.0" if FF fails to engage on Metal).
  - `retroarch.cfg`: header version stamps bumped to v3.3; all
    65 key values otherwise byte-identical to v3.2.
  - README §7 Latency: Fast Forward row updated.
  - README §7 Additional: Audio Latency, Audio Sync, Max
    Swapchain notes flagged Tier 2 per-core pins.
  - README §7 Hotkeys callout: Tier 2 `autosave_interval=0` noted.
  - README §9 Mupen + PCSX rows: rewritten for v3.3 pins.
  - Version badge 3.2 → 3.3. Companion bumped to v3.3
    (Mupen.cfg 5→9 keys, PCSX.cfg 7→9 keys, Mupen.opt 1 value
    change; total cfg 49→55, opt unchanged at 27).

2026-04-19  Ryan Musante

- v3.2: stale-reference cleanup; companion ships .opt enum fixes.
  - `retroarch.cfg`: header stamps bumped to v3.2; 65 keys
    byte-identical to v3.1.
  - README §7 Video: removed stale Mupen `swapchain=3` clause
    (pin was dropped in companion v1.57).
  - README §7 Latency: removed stale "PCE Fast = 1 (CDROM)"
    clause (PCE moved to 2-frame parity in companion v2.95).
  - Version badge 3.1 → 3.2. Companion bumped to v3.2
    (`mGBA.opt` `mgba_color_correction` "Game Boy Advance" → "Auto"
    [HIGH; enum mismatch silently disabled correction since
    introduction]; `Genesis Plus GX.opt` `genesis_plus_gx_audio_filter`
    "off" → "disabled" [correctness; same effect]).

2026-04-19  Ryan Musante

- v3.1: 3 `retroarch.cfg` value changes + README realignment.
  - `retroarch.cfg`: `fastforward_ratio` "4.0" → "0.0" (uncap;
    revert to "4.0" if FF fails on Metal).
  - `retroarch.cfg`: `audio_resampler_quality` "2" → "1" (sinc-lowest;
    imperceptible at 48 kHz).
  - `retroarch.cfg`: `savestate_thumbnail_enable` "true" → "false"
    (skips PNG encode per save).
  - `retroarch.cfg`: section dividers added; 65 keys preserved.
  - README: landing paragraph reworded for actual cfg state.
  - README §7 Video: Shader Preset row removed; Shaders row
    notes reworded (pipeline on, no global preset).
  - README §7 Latency: Run Ahead frame count corrected to "2";
    FF row marked Uncapped.
  - README §7 Additional: Resampler Quality, State Thumbnails,
    Audio Latency rows updated; Rate Control Delta + Input Max
    Users rows removed (not pinned in cfg); Security and Cloud
    Sync rows reworded to actual pinned keys.
  - README §7 Hotkeys: `autosave_interval` reference corrected
    "300" → "150".
  - README §8 Shaders: rewritten for per-core Save Core Preset
    workflow; `crt-easymode.slangp` = "Recommended starting point".
  - README §11 Checklist: shader-pipeline-on verification line.
  - Version badge 3.0 → 3.1. Companion bumped to v3.1
    (5 .opt files modified; 27 opt keys / 49 cfg keys unchanged).

2026-04-19  Ryan Musante

- v3.0: MAJOR — version-sync release.
  - Establishes lockstep versioning: both `retroarch-appletv4k`
    and `retroarch-configs` share a single MAJOR.MINOR tag from
    here forward.
  - Pre-sync: appletv4k v2.95, configs v1.57.
  - No `retroarch.cfg` or .cfg/.opt content changes — file
    contents byte-identical to v2.95 / companion v1.57.
  - README §13 Versioning: rewritten to document lockstep
    convention; MAJOR clause expanded to include cross-repo
    version-sync events.
  - Version badge 2.95 → 3.0. Companion bumped to v3.0.
