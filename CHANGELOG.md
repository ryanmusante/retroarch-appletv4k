# 5.4 - 2026-09-05

  - v5.4: completeness release - additions only, no removals. retroarch.cfg
    74 keys unchanged. Lockstep with companion retroarch-configs v5.4.
  - README.md: Systems table gains a System Name (Manual Scan) column with
    the libretro-database names of all 8 systems; new Manual Scan
    subsection (Content Directory / System Name / Default Core / Start
    Scan, File Extensions split for pce/, Scan Inside Archives off for
    arcade, FBNeo Arcade-only DAT + Arcade DAT Filter, prefer .chd).
  - README.md: new Troubleshooting section - 9 symptom -> fix rows, each
    pointing at the owning section. Quick Start step 1 names the IP source
    (tvOS Settings -> Network), step 3 the relaunch cue (XMB Gray Dark),
    step 5 links Manual Scan. File Transfers gains Linux clients (Dolphin
    webdav://, GNOME Files dav://, curl -T); tree gains playlists/ and
    screenshots/; purge list gains playlists. Shaders: Remove Core Preset.
    Badge 5.3 -> 5.4.
  - Verified: all System Name strings against libretro-database/rdb; DAT
    file name against FBNeo/dats; menu labels (Manual Scan, System Name,
    Default Core, File Extensions, Arcade DAT File / Filter, Scan Inside
    Archives, Start Scan, Remove Core Preset) against intl/msg_hash_us.h
    @v1.22.2.
  - README.md: byte size 14927; line count 282; cfg key count 74 unchanged.
  - retroarch.cfg: header + paired stamps v5.3 -> v5.4; body byte-identical.
  - Companion v5.4: 7 `.cfg` header + paired stamps v5.3 -> v5.4; 7 `.opt`
    byte-identical; README gains a core-options check step, a Systems
    cross-link, the global_core_options note and per-game .opt / removal
    paths.
  - CHANGELOG.md: trim v4.6 per 5-release retention; retained entries are
    now v5.0-v5.4.


# 5.3 - 2026-09-05

  - v5.3: audit release against upstream source (RetroArch v1.22.2 tag and
    master) and live GitHub issue state. retroarch.cfg 74 keys unchanged.
    Lockstep with companion retroarch-configs v5.3.
  - README.md: upload target corrected to /config/ - the tvOS build keeps
    retroarch.cfg at <cache>/RetroArch/config/retroarch.cfg while the web /
    WebDAV root is <cache>/RetroArch (WebServer.m, ui_cocoatouch.m).
    Directory tree redrawn: saves/, states/ and shaders/ sit at the root per
    platform_darwin.m defaults, not under config/.
  - README.md: Close Content writes one auto-save state to slot Auto
    (.state.auto); auto-index and the 10-state cap govern manual saves only.
    Hotkey row, save prose and the cfg summary Saves row corrected.
  - README.md: Configuration - tvOS reserves the controller Home button
    (Apple developer forum thread 715012); tvOS default combo Down + Y + L1
    + R1 named; NSUserDefaults mirror refreshes only on Save Current
    Configuration; re-uploading the 74-key file drops saved binds and
    directory choices. Storage Persistence and Quick Start step 4 follow.
  - README.md: Controllers - backing out of the menu root backgrounds the
    app on any pad (#18286 maintainer reply); Switch Pro row Avoid ->
    Caution. Tuning: video_hdr_enable has no effect on the v1.22.x Metal
    driver (HDR output lands on master as video_hdr_mode). Security row
    notes the network command interface enabled on tvOS in 1.22.1; Menu row
    names theme 20 (Gray Dark); playlists appear as XMB tabs; server table
    uses <atv-ip> / <device-name>.local. Badge 5.2 -> 5.3.
  - Verified: all 74 keys present in configuration.c @v1.22.2; combo 2 =
    L3 + R3, aspect 22 = Core provided, integer scaling 1 = Overscale;
    ports 80 / 8080 and no auth in WebServer.m; issues #16685, #18447,
    #18286 open, #14201 closed; both shader presets present upstream.
  - README.md: byte size 12011; line count 243; cfg key count 74 unchanged.
  - retroarch.cfg: header + paired stamps v5.2 -> v5.3; header upload path
    corrected to /config/; body byte-identical.
  - Companion v5.3: 7 `.cfg` header + paired stamps v5.2 -> v5.3; 7 `.opt`
    byte-identical; README Quick Start, Layout and override-table wording.
  - CHANGELOG.md: trim v4.5 per 5-release retention; retained entries are
    now v4.6-v5.3.


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
