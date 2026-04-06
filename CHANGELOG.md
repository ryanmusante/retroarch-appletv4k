retroarch-appletv4k changelog


2026-04-05  Ryan Musante

- Tagged as v2.30

- retroarch.cfg: add savestate_file_compression,
    savefile_file_compression, playlist_compression,
    run_ahead_hide_warnings (65 → 69 active keys before
    removals); remove 5 redundant tvOS defaults:
    video_vsync, video_crop_overscan, audio_max_timing_skew,
    fps_show, statistics_show (69 → 65 active keys, net +4 −5).
- retroarch.cfg: document PR #17093 preemptive frames /
    runahead mode relationship; merge PAL timing note into
    audio_rate_control_delta comment.
- README.md: §7 additional settings: remove FPS/Statistics
    row (now tvOS default); add compression and runahead
    warning rows; §12 checklist: remove manual compression
    step (now applied by cfg).

- Tagged as v2.29

- Sync README §7, §11 video_threaded with retroarch.cfg;
    include interpreter-bound cores (N64, PS1, DS) alongside
    GL-based cores (PPSSPP).
- retroarch.cfg: distinguish per-core overrides from Tier 3
    manual guidance in audio_latency comment.

- Tagged as v2.21 – v2.28

- retroarch.cfg: add savestate_auto_index, block_sram_overwrite
    (64 → 66 keys); remove 8 redundant tvOS defaults (72 → 64);
    fix video_threaded comment to include interpreter-bound cores.
- README.md: consolidate §7, §10, §11, §12; merge PPSSPP
    known issues; remove redundant checklist items; add overscale
    references for Genesis; update tvOS 26 and Azahar 2125.0.1;
    fix melonDS DS key prefix melondsds_ → melonds_; add .opt
    file references and retroarch-configs repo link.

2026-04-04  Ryan Musante

- Tagged as v2.20

- README.md: §10: move per-core override values to companion
    retroarch-configs repo; add Azahar 2125.0 (3DS);
    NDS BIOS Required → Optional (HLE).

2026-04-03  Ryan Musante

- Tagged as v2.19

- retroarch.cfg: add video_threaded, audio_output_rate,
    audio_resampler_quality, savestate_max_keep (68 → 72 keys).
- Remove config/ directory (maintained in retroarch-configs).
- README.md: add §4 Filesystem layout.

2026-04-02  Ryan Musante

- Tagged as v2.17, v2.18

- Add per-core override config files for Tier 1–2 (9 cores).
- retroarch.cfg: add 23 keys (45 → 68); add SECURITY and
    LOGGING sections; correct video_refresh_rate to 59.940060.

2026-04-01  Ryan Musante

- Tagged as v2.14 – v2.16

- Fact-check corrections; add MIT LICENSE.
- retroarch.cfg: add DRC, cloud sync, savestate keys (41 → 45).

2026-03-31  Ryan Musante

- Tagged as v2.11 – v2.13

- retroarch.cfg: add 13 keys (28 → 41); replace Yabause with
    Beetle Saturn.
- README.md: restructure 14 → 13 sections; add badges.

2026-03-29 – 2026-03-30  Ryan Musante

- Tagged as v2.0 – v2.10

- Convert PDF to GitHub markdown; retarget to 64 GB Wi-Fi model.
- retroarch.cfg: iterative tuning (25 → 28 keys).

2026-03-28  Ryan Musante

- Tagged as v1.0

- Initial drop-in configuration (25 active keys, 9 sections).
- Source document: retroarch-apple-tv-setup-guide.pdf.
