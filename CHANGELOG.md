# 5.2 - 2026-09-05

  - v5.2: final audit release. retroarch.cfg 74 keys unchanged. Lockstep with
    companion retroarch-configs v5.2.
  - README.md: File Transfers warning regains its consequence (anyone on the
    LAN can read or overwrite saves, states and configuration); Quick Start
    step 5 regains where playlists appear; Shaders regains the Shader
    Parameters path.
  - README.md: Tuning gains `video_refresh_rate` (60 Hz seed; calibrate via
    Settings -> Video -> Output); the cfg summary marks preemptive frames as
    mutually exclusive with run-ahead. Badge 5.1 -> 5.2.
  - Verified: every cited key / value matches retroarch.cfg; section key
    counts sum to 74; Systems core names match the companion's file names.
  - README.md: byte size 10790; line count 233; cfg key count 74
    unchanged.
  - retroarch.cfg: header + paired stamps v5.1 -> v5.2; body unchanged.
  - Companion v5.2: 7 `.cfg` header + paired stamps v5.1 -> v5.2; 7 `.opt`
    byte-identical; README File roles path corrected, inherited-keys line
    and DMC / FrameDuping rationale restored.
  - CHANGELOG.md: trim v4.4 per 5-release retention; retained entries are
    now v4.5-v5.2.


# 5.1 - 2026-09-05

  - v5.1: audit release - restores v5.0 removals judged vital and closes one
    long-standing doc gap. retroarch.cfg 74 keys unchanged. Lockstep with
    companion retroarch-configs v5.1.
  - README.md: Configuration - the Menu Toggle combo is documented as shipped
    (`input_menu_toggle_gamepad_combo = "2"` = L3 + R3, per the upstream
    enum) and the hotkey table gains the binding step it always needed:
    upstream retroarch.cfg leaves `input_enable_hotkey_btn` and every pad
    hotkey unset, so bind Hotkey Enable + the table, then Save Current
    Configuration (`config_save_on_exit = "false"` would otherwise drop
    them on quit).
  - README.md: Configuration regains the save-behaviour line (5-min SRAM
    flush, state auto-save on Close Content, 10 slots, no auto-load) and
    gains a "What retroarch.cfg sets" table - one row per cfg section, key
    counts summing to 74. Tuning gains `fps_show`; TV output regains Reduce
    Loud Sounds.
  - README.md: Systems table regains the file-extension column;
    Prerequisites regains the tvOS 13 App Store floor; Quick Start step 4
    points at binding hotkeys.
  - README.md: every cited key / value verified against retroarch.cfg.
    Badge 5.0 -> 5.1.
  - README.md: byte size 10446; line count 230; cfg key count 74
    unchanged.
  - retroarch.cfg: header + paired stamps v5.0 -> v5.1; body unchanged.
  - Companion v5.1: 7 `.cfg` header + paired stamps v5.0 -> v5.1; 7 `.opt`
    byte-identical; README regains the PCE #127 hazard, the Mupen
    `cached_interpreter` drift-guard and thread fallbacks, and the typical
    per-game overclock values.
  - CHANGELOG.md: trim v4.3 per 5-release retention; retained entries are
    now v4.4-v5.1.


# 5.0 - 2026-09-05

  - v5.0: MAJOR - README trimmed to vital information; sections merged and
    removed, so inbound anchors change. retroarch.cfg 74 keys unchanged.
    Lockstep with companion retroarch-configs v5.0.
  - README.md: Installation merged into Quick Start; ROM and BIOS Setup,
    Supported Systems and the BIOS table merged into one Systems table; Known
    Issues folded into Controllers; Contents, Files in This Repository and
    Related removed; Video / Latency / Additional settings tables replaced by
    one Tuning table of user-facing knobs. Sections: Quick Start, Storage
    Persistence, File Transfers, Systems, Controllers, Configuration, Shaders,
    Versioning, License. Badge 4.6 -> 5.0.
  - README.md: byte size 7978; line count 200; cfg key count 74
    unchanged.
  - retroarch.cfg: header reduced to name, version, target, key count,
    pairing and the upload / Save Current Configuration trap; section
    markers reduced to names. Stamps v4.6 -> v5.0. Key lines byte-identical.
  - CHANGELOG.md: retained entries v4.3-v4.6 condensed to changed keys, files,
    stamps and lockstep; rationale prose dropped, nothing renumbered or
    redated. Trim v4.2 per 5-release retention.


# 4.6 - 2026-09-05

  - README.md: all 16 `<details>` blocks gain `open`. Badge 4.5 -> 4.6.
  - retroarch.cfg: header + paired stamps v4.5 -> v4.6; 74 keys unchanged.
    Lockstep with retroarch-configs v4.6. CHANGELOG: trim v4.1.


# 4.5 - 2026-09-05

  - README.md: every table in a default-collapsed `<details>` block (9 -> 16);
    ghost-input note folded into Known Issues; FBN note, lockstep restatements
    and the CHANGELOG pointer trimmed. Badge 4.4 -> 4.5.
  - retroarch.cfg: header + paired stamps v4.4 -> v4.5; 74 keys unchanged.
    Lockstep with retroarch-configs v4.5. CHANGELOG: trim v4.0.
