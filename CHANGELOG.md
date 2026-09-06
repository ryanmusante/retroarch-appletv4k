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


# 4.4 - 2026-09-05

  - README.md: the v4.3 "tvOS does not provide Vulkan" claim corrected -
    RetroArch tvOS ships MoltenVK Vulkan (the Apple default driver), so
    `video_driver = "metal"` is a real flip and only `vulkan` can drive
    ParaLLEl-RDP. Menu paths follow the v1.22.2 tree; App Store developer
    named as Libretro; tvOS 26 target, App Store floor 13. Companion badge and
    Related section added. Badge 4.3 -> 4.4.
  - retroarch.cfg: header + paired stamps v4.3 -> v4.4; Security and Netplay
    section comments state purpose only. 74 keys unchanged. Lockstep with
    retroarch-configs v4.4. CHANGELOG: kernel.org reflow; trim v3.28.


# 4.3 - 2026-08-30

  - README.md: FBN `rewind_enable` note reclassified as drift-guard (#16374
    closed); Mupen note - `angrylion-multithread` is a worker-thread count,
    not CPU affinity; `video_hdr_enable` valid through v1.22.x, master uses
    `video_hdr_mode`. Badge 4.2 -> 4.3.
  - retroarch.cfg: header + paired stamps v4.2 -> v4.3; 74 keys unchanged.
    Lockstep with retroarch-configs v4.3. CHANGELOG: trim v3.27.
