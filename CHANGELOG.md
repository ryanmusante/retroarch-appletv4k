2026-04-25  Ryan Musante

- v3.22: PlayStation 1 / PCSX-ReARMed core retired; shader recommendations
  trimmed to primary set. 0 key-value changes in retroarch.cfg.
  * retroarch.cfg: bump header stamp v3.21 -> v3.22; 73 keys byte-identical.
  * README.md: §1 BIOS files row drops "PS1" from the requires-BIOS list.
  * README.md: §4 Filesystem layout tree drops `psx/` ROMs subfolder.
  * README.md: §5 ROM folder reference drops PlayStation 1 row.
  * README.md: §5 BIOS files table drops PlayStation 1 row; prose drops
    `scph5501.bin` case-sensitivity example (no longer applicable).
  * README.md: §7 Video table Integer Overscale row drops
    "PCSX (variable 256-640)" mention.
  * README.md: §7 Audio table -- Audio Latency row drops "PCSX mirrors
    `48` (drift-guard)" clause; Resampler Quality row drops "PS1
    44100 Hz" example; Audio Sync row drops "PCSX inherits global"
    clause.
  * README.md: §8 Recommended presets table trimmed from 9 shaders
    to 3 primary CRT shaders -- `crt-easymode.slangp` (recommended
    starting point, single-pass + integer-scale safe), `crt-aperture.
    slangp` (aperture-grille classic), `crt-geom.slangp` (curvature
    classic). Drops Minimal-cost row entries (`zfast_crt`, `crt-pi`,
    `crt-potato-warm/cool`), additional Low-cost variants
    (`crt-hyllian`, `fakelottes`), and Medium `crt-lottes-fast`.
    "Apply crt-easymode" follow-up paragraph rewritten to drop
    Minimal-preset fallback list. Integer Scaling Conflict callout
    drops `crt-hyllian` from multi-pass examples and `zfast_crt` /
    `crt-pi` from simple-shader examples (only `crt-geom` /
    `crt-easymode` / `crt-aperture` remain referenced).
  * README.md: §9 Supported Systems table drops Tier 2 PlayStation 1
    row (Mupen64Plus-Next is now sole Tier 2 entry); Mupen row gains
    v3.21 `pak1/2/3/4 = "rumble"` mention.
  * README.md: badge 3.21 -> 3.22.
  * CHANGELOG.md: trim v3.17 entry per 5-release retention; retained
    entries are now v3.18-v3.22.
  * Companion v3.22: cores 8 -> 7. `config/PCSX-ReARMed.cfg` deleted
    (-8 keys); `config/PCSX-ReARMed.opt` deleted (-6 keys); 7
    surviving `.cfg` paired stamps v3.21 -> v3.22; README cores
    badge 8 -> 7; §1 supported cores table drops PCSX row (Mupen
    row updated for v3.21 pak1-4 rumble); §2 file structure tree
    drops 2 PCSX entries; §2 ZIP-ships line "8/8" -> "7/7"; §4
    Frontend Override Keys table de-PCSX'd across 9 rows
    (`run_ahead_enabled`, `run_ahead_secondary_instance`,
    `audio_latency`, `audio_sync`, `autosave_interval`,
    `video_scale_integer` row dropped entirely as no longer
    applicable, `video_scale_integer_scaling`,
    `video_frame_delay_auto`, `rewind_enable`); §7 install tree
    drops PCSX-ReARMed/ block; §8 Overclocking drops `psxclock`
    paragraph entirely; CHANGELOG trim v3.17 per matching 5-release
    retention.
  * cfg 30 -> 22, opt 31 -> 25, cfg+opt 61 -> 47.

2026-04-25  Ryan Musante

- v3.21: behaviour flip + Mupen 4P rumble parity + 3 DiD restorations.
  70 -> 73 keys (cumulative -4 since v3.18).
  * retroarch.cfg: bump header stamp v3.20 -> v3.21; "70 keys" ->
    "73 keys".
  * retroarch.cfg: flip `menu_pause_libretro = "false"` -> `"true"`.
    Reverts to upstream default. v3.5 rationale ("preserve Run Ahead
    catch-up + FF engagement through menu open") was non-functional:
    Run Ahead operates on gameplay frames only -- irrelevant during
    menu; FF is a held hotkey -- not entered through Quick Menu.
    Pausing during menu reduces thermal load on passive A15, silences
    audio cleanly behind menu, gives deterministic save-state
    capture/restore at the paused frame. No interaction with Tier 2
    stutter mitigations (audio_sync, FrameDuping operate inside cores
    during gameplay only). Closes the longest-standing MED finding
    in the v3.18 audit.
  * retroarch.cfg: restore `network_cmd_enable = "false"` (dropped
    v3.20). Drift-guard on UDP cmd port 55355 listener -- explicit
    pin protects against imported config flipping the surface.
  * retroarch.cfg: restore `network_remote_enable = "false"` (dropped
    v3.20). Drift-guard on RA network gamepad remote path.
  * retroarch.cfg: restore `netplay_use_mitm_server = "false"`
    (dropped v3.20). Drift-guard on libretro MITM relay routing if
    user accidentally engages netplay UI.
  * Surviving Security/Netplay block: 7 keys total. 3 restored
    drift-guards (above) + 2 active hardening flips
    (`netplay_public_announce`, `netplay_nat_traversal` both flip
    upstream `true`->`false`) + 2 functional pins
    (`network_on_demand_thumbnails` per #17242,
    `cloud_sync_enable` master gate). Platform-absent surfaces
    (`stdin_cmd_enable`, `camera_allow`, `location_allow`,
    `discord_allow`) stay removed -- those keys never engage on
    tvOS regardless of value.
  * README.md: §7 Hotkeys table `menu_pause_libretro` row rewritten
    ("false"->"true" + new rationale + Tier 2 mitigation note).
  * README.md: §7 Additional settings preamble updated for
    v3.19/v3.20/v3.21 cumulative history.
  * README.md: §7 Security table adds 3 v3.21-restored rows
    (`network_cmd_enable + network_remote_enable` combined,
    `netplay_use_mitm_server` standalone).
  * README.md: badge 3.20 -> 3.21.
  * CHANGELOG.md: trim v3.16 entry per 5-release retention; retained
    entries are now v3.17-v3.21.
  * Companion v3.21: 8 `.cfg` stamps v3.20 -> v3.21 (header stamps
    and "paired with retroarch-appletv4k" pairing stamps; bodies
    byte-identical to v3.20). `Mupen64Plus-Next.opt` 7 -> 10 keys:
    add `mupen64plus-pak2/-pak3/-pak4 = "rumble"` for 4P N64 rumble
    parity (Mario Kart 64, Mario Party 1-3, GoldenEye, Perfect Dark,
    ProAm 64). pak1-4 enum {rumble, memory, transfer, none} all
    parsed identically by `mupen64plus-libretro-nx libretro.c
    update_controllers()` L757-820 (rumble->PLUGIN_RAW). Per-game
    overrides retain pak swap (e.g. Pokémon Stadium needs transfer
    pak). README badge 3.20 -> 3.21; CHANGELOG trim v3.16.
  * cfg 30, opt 28 -> 31, cfg+opt 58 -> 61.

2026-04-25  Ryan Musante

- v3.20: defense-in-depth trim continued; 4 more drift-guard pins removed.
  74 -> 70 keys (cumulative -7 since v3.18).
  * retroarch.cfg: bump header stamp v3.19 -> v3.20; "74 keys" ->
    "70 keys".
  * retroarch.cfg: remove `network_cmd_enable = "false"` -- upstream
    default already `false` per `def_v1220.h DEFAULT_NETWORK_CMD_ENABLE
    false`; UDP cmd port 55355 listener never opens unless this flips
    `true`; drift-guard with no functional effect.
  * retroarch.cfg: remove `network_remote_enable = "false"` -- upstream
    default `false` per `cfg_v1220.c:2240` (4th SETTING_BOOL arg);
    network gamepad remote path never opens unless this flips; key
    has TODO marker upstream but tvOS lacks the relevant binding code.
  * retroarch.cfg: remove `netplay_use_mitm_server = "false"` -- upstream
    default `false` per `def_v1220.h DEFAULT_NETPLAY_USE_MITM_SERVER
    false`; only relevant during active netplay session with MITM relay
    selection; pure pin.
  * retroarch.cfg: remove `discord_allow = "false"` -- upstream default
    `false` per `cfg_v1220.c:2247` (4th SETTING_BOOL arg); Discord Rich
    Presence requires desktop overlay infrastructure absent on tvOS;
    pure pin.
  * Surviving Security/Netplay block: 4 keys total. Two retained
    Netplay pins (`netplay_public_announce`, `netplay_nat_traversal`)
    flip upstream `true -> false` -- real hardening (lobby.libretro.com
    broadcast suppression + UPnP/NAT-PMP probe block). Two retained
    Security pins (`network_on_demand_thumbnails` per #17242 hang
    mitigation, `cloud_sync_enable` master gate) keep substantive
    functional rationale.
  * Surviving driver=null block: 6 keys (`bluetooth_driver`,
    `wifi_driver`, `midi_driver`, `record_driver`, `camera_driver`,
    `location_driver`) all retained -- these actively skip driver
    init code paths (e.g. `record_driver` would otherwise default to
    `wav` or `ffmpeg` per `def_v1220.h:597-599`); not pure DiD.
  * README.md: §7 preamble rewritten to disclose cumulative v3.19 +
    v3.20 trim with full key-by-key rationale and platform-absence
    citation. §7 table drops 3 rows: "Network Command Surfaces",
    "Netplay MITM Server", "Discord RPC". Retained Netplay rows
    annotated as "real hardening" (vs the dropped drift-guards).
  * README.md: badge 3.19 -> 3.20.
  * CHANGELOG.md: trim v3.15 entry per 5-release retention; retained
    entries are now v3.16-v3.20.
  * Companion v3.20: 8 `.cfg` stamps v3.19 -> v3.20 (header stamps
    and "paired with retroarch-appletv4k" pairing stamps; bodies
    byte-identical to v3.19); 8 `.opt` files unchanged (no version
    stamps per v3.12 design); README badge 3.19 -> 3.20; CHANGELOG
    trim v3.15 per matching 5-release retention.
  * cfg 30, opt 28, cfg+opt 58 -- unchanged.

2026-04-25  Ryan Musante

- v3.19: defense-in-depth trim; 3 redundant drift-guard pins removed.
  77 -> 74 keys.
  * retroarch.cfg: bump header stamp v3.18 -> v3.19; "77 keys" ->
    "74 keys".
  * retroarch.cfg: remove `stdin_cmd_enable = "false"` -- upstream
    default already `false` per `def_v1220.h` `DEFAULT_STDIN_CMD_ENABLE
    false`; tvOS RA has no stdin/console path (the stdin command
    interface is a desktop-only feature); pin was triple-redundant
    (default match + platform-absent surface + paired with no
    optimization story). Pure noise on tvOS.
  * retroarch.cfg: remove `camera_allow = "false"` -- upstream default
    already `false` per `cfg_v1220.c:2244` (4th SETTING_BOOL arg);
    Apple TV 4K 3rd Gen hardware has no camera; `camera_driver = "null"`
    already pins skip-init path on line 88. Triple-redundant.
  * retroarch.cfg: remove `location_allow = "false"` -- upstream default
    already `false` per `cfg_v1220.c:2245`; RA on tvOS does not invoke
    Core Location in any code path; `location_driver = "null"` already
    pins skip-init on line 89. Triple-redundant.
  * Surviving Security block: `network_cmd_enable`, `network_remote_enable`
    (port 55355 UDP cmd surfaces), `network_on_demand_thumbnails` (#17242
    hang mitigation), `cloud_sync_enable` (master gate). All retain
    substantive rationale.
  * README.md: §7 Additional settings table drops the "stdin Command,
    Camera, Location" Security row entirely; preamble rewritten to
    disclose v3.19 removals + rationale (upstream defaults match +
    Apple TV 4K hardware lacks underlying surfaces).
  * README.md: badge 3.18 -> 3.19.
  * CHANGELOG.md: trim v3.14 entry per 5-release retention; retained
    entries are now v3.15-v3.19.
  * Companion v3.19: 8 `.cfg` stamps v3.18 -> v3.19 (header stamps
    and "paired with retroarch-appletv4k" pairing stamps; bodies
    byte-identical to v3.18); 8 `.opt` files unchanged (no version
    stamps per v3.12 design); README badge 3.18 -> 3.19; CHANGELOG
    trim v3.14 per matching 5-release retention.
  * cfg 30, opt 28, cfg+opt 58 -- unchanged.

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

