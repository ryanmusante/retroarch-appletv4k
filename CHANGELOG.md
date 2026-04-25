2026-04-24  Ryan Musante

- v3.18: doc-correctness sync; 0 key-value changes.
  * retroarch.cfg: bump header stamp v3.17 -> v3.18; 77 keys byte-identical.
  * README.md: §7 Hotkeys callout `autosave_interval` "150" -> "300";
    "every 2.5 min" -> "every 5 min" -- claim was stale against the
    shipped global value (300 s = 5 min) since v3.5 raised the
    interval; the 150 s figure traced to a pre-v3.5 draft and never
    re-synced when the cfg was bumped.
  * README.md: §7 Additional settings adds Menu row for
    `menu_enable_widgets = "true"` -- drift-guard pin on upstream
    default, gates the OSD widget surface used by task notifications
    per v3.13 `fps_show` rationale clarification; was the only key in
    `retroarch.cfg` previously undocumented in §7.
  * README.md: badge 3.17 -> 3.18.
  * CHANGELOG.md: trim v3.13 entry per 5-release retention; retained
    entries are now v3.14-v3.18.
  * Companion v3.18: 8 `.cfg` stamps v3.17 -> v3.18 (header stamps and
    "paired with retroarch-appletv4k" pairing stamps; bodies byte-
    identical to v3.17); 8 `.opt` files unchanged (no version stamps
    per v3.12 design); README §4 Frontend Override Keys
    `autosave_interval` row rationale "every 2.5 min" -> "every 5
    min" matching companion global; README badge 3.17 -> 3.18;
    CHANGELOG trim v3.13 per matching 5-release retention.
  * cfg 30, opt 28, cfg+opt 58 -- unchanged.

2026-04-24  Ryan Musante

- v3.17: README structural trim; 0 key-value changes.
  * README.md: remove `#### Baseline (v3.16 · 77 keys)` H4 section
    (8 bullets covering Metal-path latency, command-surface/HDR
    hardening, XMB animation kill, A15 appearance pins, FPS+task
    notifications, 60 Hz SDR seed, shader pipeline, Run Ahead) and
    its trailing "See [CHANGELOG]" pointer -- per-key documentation
    retained in §7 Configuration. Standalone `[changelog]` link
    after Quick Start preserved.
  * README.md: badge 3.16 -> 3.17.
  * retroarch.cfg: bump header stamp v3.16 -> v3.17; 77 keys byte-identical.
  * CHANGELOG.md: trim v3.12 entry per 5-release retention; retained
    entries are now v3.13-v3.17.
  * Companion v3.17: 8 `.cfg` stamps v3.16 -> v3.17 (header stamps
    and "paired with retroarch-appletv4k" pairing stamps; bodies
    byte-identical to v3.16); 8 `.opt` files unchanged (no version
    stamps per v3.12 design); README badge 3.16 -> 3.17; CHANGELOG
    trim v3.12 per matching 5-release retention.
  * cfg 30, opt 28, cfg+opt 58 -- unchanged.

2026-04-24  Ryan Musante

- v3.16: doc-correctness sync; 0 key-value changes.
  * retroarch.cfg: bump header stamp v3.15 -> v3.16; 77 keys byte-identical.
  * README.md: §7 Audio table `audio_sync` row corrected -- "v3.3 Mupen
    pins false" rationale was stale since v3.9 flip to "true"; row now
    documents v3.9 flip with DRC pitch-shift rationale matching §9
    Mupen row. Resolves §7-vs-§9 internal contradiction.
  * README.md: §7 Hotkeys callout `savestate_auto_load` reworded --
    key is absent from `retroarch.cfg` (upstream default `false`);
    prior phrasing implied a configured pin among actually-pinned
    siblings.
  * README.md: badge 3.15 -> 3.16; baseline header label
    `(v3.15 · 77 keys)` -> `(v3.16 · 77 keys)`.
  * CHANGELOG.md: trim v3.11 entry per 5-release retention; retained
    entries are now v3.12-v3.16.
  * Companion v3.16: 8 `.cfg` stamps v3.15 -> v3.16; README §7 path
    frame aligned to WebDAV-relative `config/<core>/` -- was
    `Documents/RetroArch/config/<core>/` (native sandbox path that
    would resolve to a wrong location if followed literally via the
    WebDAV transfer flow documented in companion §4); cfg 30, opt 28,
    cfg+opt 58 -- unchanged.

2026-04-24  Ryan Musante

- v3.15: upstream-correctness pass; 6 dead/wrong keys removed or fixed.
  82 -> 77 keys.
  * retroarch.cfg: bump header stamp v3.14 -> v3.15; "80 keys" ->
    "77 keys".
  * retroarch.cfg: remove `video_frame_rest = "1"` -- key removed in RA
    v1.20.0 per upstream `CHANGES.md` L401 ("Remove Frame Rest, obsoleted
    by Frame Delay refactor"); absent from `configuration.c`,
    `config.def.h`, `menu_setting.c` in v1.22.0 source.
  * retroarch.cfg: remove `video_hdr_contrast = "5.0"` -- key does not
    exist in RA v1.22.0; real key is `video_hdr_display_contrast` per
    `configuration.c:2356`; inert anyway with `video_hdr_enable = "false"`.
  * retroarch.cfg: remove `video_max_swapchain_images = "2"` -- Metal
    driver hardcodes `MAX_INFLIGHT 1` in `gfx/common/metal/metal_common.h`
    L30 with TODO comment "implement triple buffering"; metal.m never
    reads the setting; CAMetalLayer falls back to Apple defaults
    regardless of frontend value.
  * retroarch.cfg: `menu_xmb_animation_horizontal_highlight` "2" -> "0";
    `menu_xmb_animation_opening_main_menu` "2" -> "0". Per
    `menu/menu_setting.c` v1.22.0 L4123-4175, value 2 maps to "Easing
    Out Bounce" (range 0-2) and "Easing Out Expo" (range 0-3)
    respectively -- neither key exposes a "None" enum value. Only
    `menu_xmb_animation_move_up_down` has case 2 =
    `MENU_ENUM_LABEL_VALUE_NONE` (kept at "2"). v3.9-v3.14 set all
    three to "2", which produced the bounciest animations on two of
    them -- the opposite of intent. v3.15 pins both to "0" (Easing Out
    Quad, the upstream `DEFAULT_XMB_ANIMATION`).
  * retroarch.cfg: `audio_resampler_quality` "1" -> "2" (LOWEST ->
    LOWER per `libretro-common/include/audio/audio_resampler.h` L52-57
    enum); audible quality gain on cores not native to 48 kHz at sub-1%
    A15 CPU cost.
  * README.md: badge 3.14 -> 3.15; landing paragraph "80-key" ->
    "77-key"; baseline summary rewritten -- Metal-path latency bullet
    drops `video_frame_rest`, animation kill bullet rewritten as
    "partial" with explicit per-key None-availability disclosure;
    §7 Video drops Max Swapchain Images row; §7 Latency drops Frame
    Rest row; §7 Additional XMB Animations row rewritten with
    enum-correctness rationale; §7 HDR row drops `video_hdr_contrast`;
    §7 Resampler Quality row updated for v3.15 raise; §9 Mupen + PCSX
    rows drop `video_max_swapchain_images = 3` mention; §9 PCSX row
    notes `gpu_thread_rendering = "enabled"` v3.15 fix.
  * CHANGELOG.md: trim v3.10 entry per 5-release retention; retained
    entries are now v3.11-v3.15.
  * Companion v3.15: 8 `.cfg` stamps v3.14 -> v3.15;
    `Mupen64Plus-Next.cfg` 9 -> 8 keys (drop swapchain);
    `PCSX-ReARMed.cfg` 9 -> 8 keys (drop swapchain);
    `PCSX-ReARMed.opt` `pcsx_rearmed_gpu_thread_rendering` "async" ->
    "enabled" (was invalid value silently falling back to default
    "auto"); `PCSX-ReARMed.opt` header drops incorrect `cd_readahead`
    WARNING (V(333000) IS in the dropdown enum on tvOS per
    `pcsx_rearmed_opts.h` L186-189). cfg 30, opt 28, cfg+opt 58.

2026-04-24  Ryan Musante

- v3.14: doc format pass; GitHub-style README, kernel.org-style CHANGELOG.
  * retroarch.cfg: bump header stamp v3.13 -> v3.14; 80 keys byte-identical.
  * README.md: tighten landing paragraph (split 450-word block into intro,
    baseline summary, history pointer); badge 3.13 -> 3.14.
  * CHANGELOG.md: rewrite all retained entries in kernel.org style
    (file-first bullets, imperative mood, trimmed prose).
  * CHANGELOG.md: trim v3.9 entry per 5-release retention; retained
    entries are now v3.10-v3.14.
  * Companion v3.14: 8 `.cfg` stamps v3.13 -> v3.14; same CHANGELOG
    format pass; cfg 32, opt 28, cfg+opt 60 -- unchanged.
