2026-04-22  Ryan Musante

- v3.7: 3 documentation-only edits; 0 cfg/opt value changes.
  90 `retroarch.cfg` keys byte-identical to v3.6.

  `retroarch.cfg`: no changes.

  README:
  - §9 Tier 2 Mupen row: "(P-core pin on 2P+4E A15)" →
    "(P-core pin on 2P+3E binned A15)". Apple TV 4K 3rd Gen
    ships a binned A15 with one efficiency core fused off
    (2P+3E, 5 cores total), a distinct variant from the
    iPhone 13/14 A15 despite sharing the marketing name.
  - §6 Controllers: qualify "up to four simultaneous Bluetooth
    controllers" with tvOS RetroArch cap at three
    ([#16685](https://github.com/libretro/RetroArch/issues/16685)).
  - §10 Known Issues: new row #7 for #16685.
  - Landing paragraph: "as of v3.6" → "as of v3.7".

  Version badge 3.6 → 3.7. Companion bumped to v3.7 (README
  §1/§4 doc-only 2P+4E → 2P+3E corrections + Mupen64Plus-Next
  .cfg/.opt inline-comment corrections + CHANGELOG history
  corrections on 4 lines; all .cfg / .opt key=value lines
  byte-identical to v3.6).

  Sources: https://en.wikipedia.org/wiki/Apple_A15;
  https://www.macrumors.com/2022/11/14/new-apple-tv-5-core-cpu/

2026-04-21  Ryan Musante

- v3.6: 2 `retroarch.cfg` value changes; companion ships 1 value
  change + 1 doc-sync comment. 90 keys unchanged.

  `retroarch.cfg`:
  - `fastforward_ratio` "5.0" → "4.0" (MED; 4× matches community
    standalone-emulator norms and reduces thermal load during
    sustained FF bursts on passive A15; v3.3 5× cap was set as a
    Tier 2 audio-underrun mitigation over uncapped 0.0, which 4×
    also satisfies).
  - `audio_latency` "32" → "48" (HIGH; v3.5 32 ms was aggressive —
    crackle risk under thermal throttle and tight buffer on FBNeo
    secondary-instance workload. 48 ms ≈ 3 frames @ 60 Hz is the
    sweet spot: preserves most of the 32 benefit, matches standalone
    emulator community norms, and degrades gracefully. Tier 2 pins
    adjusted per companion v3.6 — Mupen absolute 64, PCSX mirrors
    global 48).
  - 90 keys, byte-identical to v3.5 apart from the two values and
    the header version stamps.

  README:
  - Landing paragraph: `audio_latency = "32"` → `"48"`; version
    stamp 3.5 → 3.6.
  - §7 Latency table: Fast Forward Ratio row rewritten for
    4.0 + thermal/community-norm rationale.
  - §7 Additional table: Audio Latency row rewritten for 48 +
    sweet-spot rationale; Tier 2 pin values updated (PCSX=48
    drift-guard mirror, Mupen=64).

  Version badge 3.5 → 3.6. Companion bumped to v3.6
  (`Mupen64Plus-Next.cfg` `audio_latency` "96" → "64";
  `PCSX-ReARMed.cfg` inline comment doc-synced — value "48"
  unchanged but rationale now "drift-guard mirror of v3.6
  global" instead of "tighter than global 64"; all 8 .cfg
  header "paired with" stamps v3.5 → v3.6; .opt content
  byte-identical to v3.5).

2026-04-21  Ryan Musante

- v3.5: `retroarch.cfg` latency + hardening refresh (3 modifications,
  25 additions; cfg 65 → 90 keys). Companion unchanged.

  Modifications:
  - `menu_pause_libretro` "true" → "false" (HIGH; was neutralizing
    Tier 1 Run Ahead catch-up and Fast Forward engagement on every
    menu open; core now keeps running behind Quick Menu).
  - `pause_nonactive` "true" → "false" (MED; tvOS briefly marks
    app inactive on Siri Remote wake / Control Center / HDMI CEC —
    causes audio-buffer glitch and spurious mid-run pause).
  - `audio_latency` "64" → "32" (HIGH; A15 + CoreAudio sustains
    32 ms; halves audio buffer depth. Tier 2 pins unaffected —
    PCSX 48, Mupen 96 override up. Revert to 48 or 64 if crackle
    under thermal throttle).

  Additions (25 keys):

  Menu / UI (6):
  - `core_info_cache_enable = "true"` (cold-start win on 64 GB
    purgeable-cache).
  - `menu_xmb_animation_opening_main_menu = "0"`,
    `menu_xmb_animation_horizontal_highlight = "0"`,
    `menu_xmb_animation_move_up_down = "0"` (kills XMB ribbon /
    transition GPU load; instant menu cuts).
  - `rgui_inline_thumbnails = "false"`,
    `menu_show_sublabels = "false"` (small cache + CPU save).

  Video (8):
  - `video_frame_rest = "1"` (HIGH; RA 1.17 CPU end-of-frame
    sleep; frees Metal queue for earlier present; 1–3 ms
    CPU→display reduction; fixed-refresh only).
  - `video_font_enable = "false"` (disables OSD text render;
    FPS counter / savestate notifications no longer drawn).
  - `video_black_frame_insertion = "0"` (explicit 0 for 60 Hz
    panel; drift-guard).
  - `video_bfi_dark_frames = "1"`, `video_shader_subframes = "1"`
    (defensive defaults vs future release drift; inert at 60 Hz).
  - `video_hdr_enable = "false"`, `video_hdr_max_nits = "1000"`,
    `video_hdr_contrast = "5.0"` (explicit-off guard against
    Metal HDR10 surface negotiation on DV / HDR10 TV modes;
    §7 TV output still directs user to 4K SDR).

  Input (2):
  - `input_auto_game_focus = "1"` (auto-grabs focus on content
    load; prevents tvOS Siri Remote leak into game focus).
  - `input_bind_timeout = "3"` (BT HID 125 Hz retry window;
    default 5 s frequently drops bind samples on GCController).

  Latency (1):
  - `fastforward_frameskip = "true"` (pairs with
    `fastforward_ratio = "5.0"`; drops GPU ~50% when FF engages;
    no effect at 1×).

  Security (2):
  - `network_cmd_enable = "false"`, `network_remote_enable = "false"`
    (explicit-off RA UDP command surfaces; complements existing
    `stdin_cmd_enable = "false"` pin).

  Null drivers (6):
  - `bluetooth_driver = "null"`, `wifi_driver = "null"`,
    `midi_driver = "null"`, `record_driver = "null"`,
    `camera_driver = "null"`, `location_driver = "null"`
    (tvOS-inert subsystems handled at OS level; skip RA-side
    init entirely).

  Layout:
  - `retroarch.cfg`: keys re-grouped for new sections; new
    "Null drivers" section added. Header 65 → 90.

  README:
  - §7 Latency table: new rows `video_frame_rest`,
    `fastforward_frameskip`.
  - §7 Additional table: `menu_pause_libretro` "true" → "false"
    (note rewritten); `pause_nonactive` "true" → "false";
    `audio_latency` "64" → "32" note updated; new rows for
    Menu/UI, Video, Input, Security, Null drivers additions.
  - Landing paragraph: key count 65 → 90; companion v3.5.

  Version badge 3.4 → 3.5. Companion bumped to v3.5 (SYNC;
  all 8 `.cfg` "paired with" stamps v3.4 → v3.5; .cfg/.opt
  content byte-identical to v3.4).

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
