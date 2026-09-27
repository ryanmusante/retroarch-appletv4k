# 5.6 - 2026-09-26

  - v5.6: deep-scan release against upstream source and Apple
    documentation. retroarch.cfg 74 keys unchanged. Lockstep with
    companion retroarch-configs v5.6.
  - README.md: File Transfers - the servers run only while RetroArch is in
    the foreground (GCDWebServer suspends them in the background by
    default); the security note trades the router-firewall advice, which
    cannot filter traffic between hosts on one LAN, for VLAN isolation.
  - README.md: Tuning - vrr_runloop_enable true skips the audio
    timing-skew sync and paces on video only (runloop.c @v1.22.2); the
    "disables Dynamic Rate Control" wording is withdrawn, DRC is gated by
    audio_rate_control alone (audio_driver.c).
  - README.md: Storage Persistence - tvOS may purge the cache only while
    RetroArch is not running; Controllers - tvOS allows four controllers,
    or one while a Bluetooth audio accessory is connected, with the
    Troubleshooting pad row to match; Systems - Neo Geo BIOS at
    fbneo/neogeo.zip, the core-info firmware path. Badge 5.5 -> 5.6.
  - Verified: A2737 = 64 GB Wi-Fi model without Ethernet (Apple 101605);
    500 KB NSUserDefaults limit (App Programming Guide for tvOS); pairing
    path Settings -> Remotes and Devices -> Bluetooth (tvOS User Guide);
    extensions and BIOS names against libretro-core-info; mFi autoconfig
    binds Menu Toggle to Home (button 16); zfast-crt and lcd-grid-v2 are
    single-pass; all README links resolve; section key counts sum to 74.
  - README.md: byte size 15570; line count 285; cfg key count 74 unchanged.
  - retroarch.cfg: header + paired stamps v5.5 -> v5.6; body
    byte-identical.
  - Companion v5.6: 7 `.cfg` header + paired stamps v5.5 -> v5.6; 7 `.opt`
    byte-identical; README badge only.
  - CHANGELOG.md: trim v5.1 per 5-release retention; retained entries are
    now v5.2-v5.6.


# 5.5 - 2026-09-26

  - v5.5: currency release against upstream and tvOS 27. retroarch.cfg 74
    keys unchanged. Lockstep with companion retroarch-configs v5.5.
  - Target tvOS 26 -> 27 (GA 2026-09-14, supports Apple TV 4K 2nd and 3rd
    Gen; RetroArch confirmed running on-device). RetroArch v1.22.2
    (2025-11-20) remains the latest release. README intro, Prerequisites
    and the retroarch.cfg header updated.
  - README.md: Troubleshooting gains a lockout row - tvOS Settings -> Apps
    -> RetroArch -> Restore Default Config renames config/retroarch.cfg to
    RetroArch-HHmm-yyMMdd.cfg and clears the NSUserDefaults mirror on the
    next launch; defaults load with the Down + Y + L1 + R1 menu combo.
  - README.md: Quick Start step 2 uses the exact Online Updater labels
    (Update Assets, Update Core Info Files, Update Databases, Update Slang
    Shaders); Controllers notes the 8BitDo Pro 2 pairs in mode D; Shaders
    drops Guest-Dr-Venom, no longer a preset in libretro/slang-shaders.
    Badge 5.4 -> 5.5.
  - Verified: 74 keys in configuration.c @v1.22.2, master replaces only
    video_hdr_enable (video_hdr_mode); Restore Default Config in
    Settings.bundle/Root.plist and ui_cocoatouch.m; Update Core Info Files
    built into App Store builds (BaseConfig.xcconfig); 13 System Names in
    libretro-database/rdb; FBNeo Arcade-only DAT; zfast-crt and
    lcd-grid-v2 presets; neogeo.zip found in the system root (FBNeo
    libretro.cpp); #16685, #18447, #18286 open; tvOS withholds the pad
    Home button (Apple forum thread 715012).
  - README.md: byte size 15262; line count 282; cfg key count 74 unchanged.
  - retroarch.cfg: header + paired stamps v5.4 -> v5.5, target tvOS 27;
    body byte-identical.
  - Companion v5.5: 7 `.cfg` header + paired stamps v5.4 -> v5.5;
    Mupen64Plus-Next.opt header note corrected, other 6 `.opt`
    byte-identical; README tvOS 27, Angrylion label, Mupen driver note;
    config/.gitkeep removed.
  - CHANGELOG.md: trim v5.0 per 5-release retention; retained entries are
    now v5.1-v5.5.


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
