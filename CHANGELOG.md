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

2026-04-24  Ryan Musante

- v3.13: documentation-correctness pass; 0 key or structure changes.
  * retroarch.cfg: bump header stamp v3.12 -> v3.13; 80 keys byte-identical.
  * README.md: §7 Video `fps_show` row rationale corrected -- inherits
    upstream default `video_font_enable = "true"` (removed as drift-guard
    in v3.9), not an explicit pin; task notifications render via
    `menu_enable_widgets`, not `video_font_enable`.
  * README.md: §7 Menu row added for `menu_show_sublabels = "false"`
    (shipped pre-v3.5, previously undocumented).
  * README.md: §7 Logging row added for `log_verbosity = "false"`
    (shipped pre-v3.5, previously undocumented).
  * README.md: landing paragraph appends v3.13 correctness-pass sentence;
    badge 3.12 -> 3.13.
  * CHANGELOG.md: v3.12 historical `fps_show` rationale patched
    (text-only; preserves key-swap narrative and 80-key count).
  * Companion v3.13: 8 `.cfg` stamps v3.12 -> v3.13; README §4
    `audio_latency` row drops ambiguous "pre-v3.5 was tighter-than-
    global 64" clause (pre-v3.5 global was also 64).

2026-04-24  Ryan Musante

- v3.12: retroarch.cfg net-zero key swap; 80 keys unchanged.
  * retroarch.cfg: add `fps_show = "true"` (on-screen FPS counter;
    renders via OSD text path, upstream default `video_font_enable =
    "true"`; no additional GPU cost).
  * retroarch.cfg: remove `netplay_start_as_spectator = "false"`
    (drift-guard matching upstream default; runtime unchanged).
  * retroarch.cfg: Netplay section header drops "spectator" mention:
    `# === Netplay (disable public announce, NAT-PMP/UPnP, MITM relay) ===`.
  * retroarch.cfg: 4-line file header collapsed to single-line form
    per v3.12 comment-style unification.
  * README.md: landing paragraph rewritten for v3.12; §7 Video adds
    On-Screen FPS row; §7 Additional Network removes Spectator row;
    badge 3.11 -> 3.12.
  * CHANGELOG.md: trim v3.7 entry per 5-release retention.
  * Companion v3.12: 8 `.cfg` stamps v3.11 -> v3.12; 16 override files'
    comment blocks collapsed to single-line form (headers, section-
    header inline rationales, WARNING blocks reduced to one line with
    `·` and `;` separators preserving content); 0 key-value changes
    across any `.cfg` or `.opt`; cfg 32, opt 28, cfg+opt 60 -- unchanged.
