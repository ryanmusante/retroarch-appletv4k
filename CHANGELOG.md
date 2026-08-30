# 4.3 - 2026-08-30

- v4.3: README accuracy pass; retroarch.cfg 74 keys byte-identical to v4.2 (header stamp only). Released in lockstep with companion retroarch-configs v4.3.
- retroarch.cfg: bump header version stamp + paired stamp v4.2 -> v4.3.
- README.md: Supported Systems — FinalBurn Neo rewind note corrected. Upstream FBNeo `src/burner/libretro/README.md` struck the #16374 note on 2026-05-12 and RetroArch #16374 is closed. The companion pin equals the global value, so it is a drift-guard, not a live workaround.
- README.md: Supported Systems — Mupen Notes reworded. `mupen64plus-angrylion-multithread` sets the RDP worker-thread count, not CPU affinity; tvOS exposes no P-core pinning to applications. "Metal-only stack" replaced with "software stack": ParaLLEl-RDP/RSP are compiled into the tvOS core but need Vulkan, which RetroArch tvOS does not provide, and GLideN64 needs a GL context that `video_driver = "metal"` does not provide.
- README.md: Configuration > Additional settings — `video_hdr_enable` row gains a forward-compatibility note. The key is valid through v1.22.x; RetroArch master replaces it with `video_hdr_mode` (`0` off, `1` HDR10, `2` scRGB). Re-check the pin when moving off the v1.22.x target.
- README.md: the v4.2 entry recorded byte size 16192 and line count 367 and a `paired` badge bump. The file carries a single version badge and neither figure matched what shipped. Corrected here rather than by editing the historical entry; see the metrics line below.
- README.md: version badge 4.2 -> 4.3.
- README.md: byte size 16076; line count 363; cfg key count 74 unchanged.
- Companion v4.3: 7 `.cfg` header + paired stamps v4.2 -> v4.3; comment corrections in FinalBurn Neo.cfg, Mupen64Plus-Next.cfg, Mupen64Plus-Next.opt; `config/.gitkeep` removed. cfg 21, opt 18, cfg+opt 39 unchanged.
- CHANGELOG.md: trim v3.27 entry per 5-release retention; retained entries are now v3.28-v4.3.

# 4.2 - 2026-07-26

- v4.2: retroarch.cfg `input_auto_game_focus` correction + README accuracy pass; 74 keys unchanged. Released in lockstep with companion retroarch-configs v4.2.
- retroarch.cfg: `input_auto_game_focus "1" -> "0"`. With Game Focus enabled, RetroArch forces it on every time the menu closes over loaded content (`menu/menu_driver.c` v1.22.2), setting `INP_FLAG_BLOCK_HOTKEY` + `INP_FLAG_KB_MAPPING_BLOCKED` (`retroarch.c`). When an Enable Hotkeys modifier is bound on the pad and no keyboard bind exists — the setup this README documents — the selective-block path in `input/input_driver.c` offers no unblock branch, so every hotkey bind stays dead while content runs: save/load state, fast-forward, state slot, Close Content. Only the L3 + R3 menu combo survived, because `runloop.c` evaluates it from raw pad bits ahead of hotkey blocking. `"0"` is the upstream default (`DEFAULT_INPUT_AUTO_GAME_FOCUS AUTO_GAME_FOCUS_OFF`).
- retroarch.cfg: bump header version stamp + paired stamp v4.1 -> v4.2.
- README.md: Configuration — Close Content warning replaced. Exiting content never required a Close Content hotkey (the Quick Menu carries the entry); the real failure mode is an unset Menu Toggle combo, which strands the user in the running core. Enable Hotkeys modifier promoted from optional to required, matching the Select-based combo table below it.
- README.md: Configuration > Hotkeys — note added that Game Focus must stay off for hotkey binds to fire.
- README.md: Configuration > Additional settings — `input_auto_game_focus` row added; table 7 -> 8 rows. Note states the trade explicitly: enabling Game Focus swaps all pad hotkeys for keyboard capture.
- README.md: Storage Persistence — asset re-extraction re-dated. Landed in RetroArch 1.18.0 ("TVOS: Force asset re-extraction when cache is deleted"), not 1.16.0; the NSUserDefaults `retroarch.cfg` mirror remains 1.16.0. Scope widened from "shader assets" to assets generally, per upstream wording.
- README.md: Configuration > TV output — "fixed 59.94 Hz panel" -> "fixed 60 Hz output". Prior figure contradicted both the shipped `video_refresh_rate = "60.000000"` and the 4K SDR 60 Hz row in the same table.
- README.md: ROM and BIOS Setup — folder names reframed as convention. Manual Scan identifies content against the system database selected in the scan dialog, not against directory names.
- README.md: Prerequisites — RetroArch row rationale narrowed to WebDAV. Auto frame delay and integer scaling long predate v1.20.0 and did not motivate the floor; v1.22.x noted as recommended.
- README.md: Configuration > Video settings — `video_smooth` note "Required for shader rendering" -> "Nearest-neighbour; avoids pre-shader blur". Bilinear filtering degrades CRT shader output but is not a hard requirement.
- README.md: Configuration > Latency reduction — `video_frame_delay_auto` Mupen pin marked drift-guard; #14201 is closed upstream and the pin now guards against regression only.
- README.md: Supported Systems — Beetle PCE Fast Notes drops "CD precache", stale since the companion removed `pce_fast_cdimagecache = "enabled"` at v4.1. Shipped behaviour is 2x CD read speed, with full-image precache retained as a per-game opt-in.
- README.md: Versioning — retention wording "last 5 MINOR entries" -> "last 5 releases"; the retained window includes the v4.0 MAJOR entry.
- README.md: version badge 4.1 -> 4.2; paired badge `retroarch--configs v4.1` -> `v4.2`.
- README.md: byte size 15657 -> 16192 (+535); line count 362 -> 367 (+5); cfg key count 74 unchanged.
- Companion v4.2: 7 `.cfg` header + paired stamps v4.1 -> v4.2, bodies byte-identical to v4.1. No per-core `input_auto_game_focus` override exists (global cfg governs), so no companion body edit required. CHANGELOG trim v3.26 per matching 5-release retention.
- CHANGELOG.md: trim v3.26 entry per 5-release retention; retained entries are now v3.27-v4.2.

# 4.1 - 2026-07-05

- v4.1: retroarch.cfg `audio_latency` tuning fix — first cfg body change since v4.0; 74 keys unchanged. Released in lockstep with companion retroarch-configs v4.1.
- retroarch.cfg: `audio_latency "48" -> "64"`. Prior "48" (2.88 frames @ 60 Hz) sat below the libretro optimal-vsync 3-frame safe floor (52 ms) documented at libretro/docs `docs/guides/optimal-vsync.md`. On the passively-cooled A15, sustained throttling induces frame-time spikes a sub-3-frame audio buffer fails to absorb. "64" (3.84 frames) clears the floor and matches the upstream default for this platform (`config.def.h` v1.22.2 `DEFAULT_OUT_LATENCY 64`, non-Android branch), trading ~1 frame of audio latency for stutter resistance. Latency-favoring "48" remains available as a per-user flip.
- retroarch.cfg: bump header version stamp + paired stamp v4.0 -> v4.1.
- README.md: Additional settings `audio_latency` row `48` -> `64`; Notes rewritten — 64 is the shipped default, 48 the documented opt-in.
- README.md: version badge 4.0 -> 4.1; paired badge `retroarch--configs v4.0` -> `v4.1`.
- README.md: byte size 15555 -> 15657 (+102); line count 362 unchanged; cfg key count 74 unchanged.
- Companion v4.1: 7 `.cfg` stamps v4.0 -> v4.1, bodies byte-identical to v4.0. Beetle PCE Fast.opt drops `pce_fast_cdimagecache = "enabled"` (reverts to upstream default `"disabled"`; multi-hundred-MB precache imprudent on the 4 GB fanless ATV4K); opt 3 -> 2 keys, cfg+opt 40 -> 39.
- CHANGELOG.md: trim v3.25 entry per 5-release retention.

# 4.0 - 2026-05-16

- v4.0: MAJOR bump — README restructured to ry-install style (breaking anchor schema); retroarch.cfg 74 keys byte-identical to v3.28 (header stamp + §2 reference fix only).
- retroarch.cfg: bump header version stamp + paired stamp v3.28 -> v4.0. Trailing `(§2)` reference replaced with `(see README Installation)` — sections are no longer numbered.
- README.md: **BREAKING** anchor schema change — slugs drop the leading `N-` prefix (`#1-prerequisites` -> `#prerequisites`). External inbound links to old anchors will 404. Motivates the MAJOR bump per versioning policy.
- README.md: full restructure to ry-install template. Numbered sections dropped (§1-§13 -> unnumbered); all `§N` references removed body-wide; `## Table of Contents` -> `## Contents`, numbered list -> bulleted list.
- README.md: 9 reference subsections folded into default-collapsed `<details>` blocks — Filesystem layout; ROM folder reference; BIOS files; Hotkeys; Video settings; Latency reduction; TV output; Additional settings; Assign procedure. File Transfers web-interface / WebDAV subsections merged into one `| Server | Port | Endpoint |` table.
- README.md: GitHub admonitions replace prose callouts — Quick Start (Save Current Config trap), Storage Persistence (500 KB cap), File Transfers (unauthenticated 80/8080), Controllers (ghost inputs), Configuration (menu access), TV output (VRR / DRC failure modes).
- README.md: Quick Start moved above Contents; header gains tvOS + paired badges (4 badges + license); License adopts `MIT © 2026 Ryan Musante`.
- README.md: cross-repo Supported Systems Mupen link `retroarch-configs#4-frontend-override-keys` -> `retroarch-configs#configuration`; Latency reduction `run_ahead_secondary_instance` Notes drops the upstream FBN attribution for a reciprocal "per companion repo" pointer.
- README.md: byte size 16240 -> 15555 (-4.2%); line count 279 -> 362 (+83).
- Companion v4.0: 7 `.cfg` paired stamps v3.28 -> v4.0; bodies byte-identical (cfg 21, opt 19, cfg+opt 40).
- CHANGELOG.md: trim v3.24 entry per 5-release retention.

# 3.28 - 2026-05-10

- v3.28: paired companion FBN secondary-instance correction + Beetle PCE Fast cdspeed rebase; retroarch.cfg 74 keys byte-identical to v3.27 (header stamp only).
- retroarch.cfg: bump header stamp v3.27 -> v3.28.
- README.md: §7 Latency reduction `run_ahead_secondary_instance` Notes updated. Per upstream FBN libretro README at libretro/FBNeo `src/burner/libretro/README.md`, runahead single instance and preemptive frames are the recommended methods and second-instance support is not guaranteed. New text: "All cores inherit; single-instance per upstream FBN libretro README".
- README.md: §9 Tier 1 FBN Notes drops "Run Ahead 2 + secondary instance" for "Run Ahead 2 single-instance"; #16374 rewind reference retained.
- README.md: badge 3.27 -> 3.28; intro April 2026 -> May 2026.
- Companion v3.28: 7 `.cfg` paired stamps v3.27 -> v3.28. FinalBurn Neo.cfg drops `run_ahead_secondary_instance = "true"` and inherits global `false`. Beetle PCE Fast.opt `pce_fast_cdspeed "4" -> "2"` per upstream `libretro_core_options.h` advisory; per-game override to "4" remains available. README FBN row keys 4 -> 3; §4 `run_ahead_secondary_instance` Purpose rewritten.
- cfg 22 -> 21, opt 19, cfg+opt 41 -> 40.
- CHANGELOG.md: trim v3.23 entry per 5-release retention.
