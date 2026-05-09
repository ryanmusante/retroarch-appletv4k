# 3.26 - 2026-05-09

- v3.26: paired companion default-matching-key trim + documentation accuracy pass; retroarch.cfg 74 keys byte-identical to v3.25 (header stamp only).
- retroarch.cfg: bump header stamp v3.25 -> v3.26.
- README.md: §7 Security `network_on_demand_thumbnails = "false"` row rationale rewritten. Prior cite `#17242 "Hangs on slow thumbnail server"` is stale — issue closed (completed) upstream. Pin retained as defense-in-depth on outbound thumbnail-server traffic; rationale now framed accordingly.
- README.md: §7 Latency reduction `Run-Ahead Mode` row — FBN `run_ahead_secondary_instance = "true"` citation reworded; `[#16374]` was the rewind+runahead conflict tracker, not the secondary-instance recommendation. Now framed as "per FBN core maintainer recommendation".
- README.md: §9 Mesen row — `mesen_overclock_rate` -> `mesen_overclock`. Prior key did not exist upstream; real key is `mesen_overclock` (enum: None / Low / Medium / High / Very High) per libretro/Mesen `Libretro/libretro_core_options.h:78`. Per-game guidance for Battletoads / Recca retained with corrected key.
- README.md: §9 SNES row — `snes9x_overclock = 200` -> `snes9x_overclock_superfx = "200%"`. Prior key did not exist; real key is `snes9x_overclock_superfx` (50%-500%; percent suffix required) per libretro/snes9x `libretro/libretro_core_options.h:192`. Star Fox / Yoshi's Island / Doom / Stunt Race FX guidance retained with corrected key + value format.
- README.md: §9 Mupen Tier 2 row — `mupen64plus-cpucore = "cached_interpreter"` reframed as a drift-guard pin against `DYNAREC`-enabled builds. tvOS App Store has `DYNAREC` undef, so `cached_interpreter` IS the upstream default per libretro/mupen64plus-libretro-nx `libretro/libretro_core_options.h:1391`. Other two stack members (Angrylion, CXD4) remain real flips.
- README.md: §10 Known Issues — issue states resynced against upstream:
  - #3 [#18300](https://github.com/libretro/RetroArch/issues/18300): Open -> Closed (completed). Per-core rewind config landed; Tier 2 `rewind_enable = "false"` pin retained as drift-guard.
  - #4 [#14201](https://github.com/libretro/RetroArch/issues/14201): Open -> Closed (completed). Mupen `video_frame_delay_auto = "false"` pin retained as drift-guard.
  - #5 [#16598](https://github.com/libretro/RetroArch/issues/16598): Open -> Closed (completed). Per-game override guidance retained for any title-specific residuals.
  - #6 [#14978](https://github.com/libretro/RetroArch/issues/14978): Persists -> Closed (force-disabled upstream). `gfx/video_driver.c` `__MACH__ && __APPLE__` runtime override remains the actual mechanism.
- README.md: §12 Files In This Repository — CHANGELOG row description "GNU ChangeLog style" -> "kernel.org style".
- README.md: §13 Versioning — changelog format documented as kernel.org style (`# Version - Date` + bullets). Prior GNU ChangeLog wording dropped.
- README.md: badge 3.25 -> 3.26.
- CHANGELOG.md: trim v3.21 entry per 5-release retention; retained entries are now v3.22-v3.26. Format converted from GNU ChangeLog (`YYYY-MM-DD<SP><SP>Author`) to kernel.org style (`# Version - Date` + bullets) across all retained entries.
- Companion v3.26: 7 `.cfg` paired stamps v3.25 -> v3.26 (5 bodies byte-identical to v3.25; FinalBurn Neo.cfg + Mupen64Plus-Next.cfg have header rewords). 1 `.opt` body edit: `mGBA.opt` 3 keys -> 1 key (drops `mgba_interframe_blending = "OFF"` and `mgba_audio_low_pass_filter = "disabled"` — both match upstream defaults at libretro/mgba `src/platform/libretro/libretro_core_options.h:208/L223`; same class as v3.25 Genesis Plus GX `audio_filter` trim). Retained: `mgba_color_correction = "Auto"` (real flip from default `"OFF"`). README §1 mGBA `.opt` count 3 -> 1; FBN row `run_ahead_secondary_instance` cite reworded; Mupen row `cpucore` reframed; §4 Frontend Override Keys table FBN `run_ahead_secondary_instance` row drops `[#16374]`; §8 Overclocking rewritten with real key names + values; §11 Versioning converted to kernel.org style. README badge 3.25 -> 3.26. CHANGELOG trim v3.21 per matching 5-release retention.
- cfg 22, opt 21 -> 19, cfg+opt 43 -> 41.

# 3.25 - 2026-05-02

- v3.25: paired companion default-matching-key trim; retroarch.cfg 73 -> 74 keys.
- retroarch.cfg: `audio_resampler_quality "2" -> "3"`. Real flip from tvOS upstream LOWER default (`config.def.h __MACH__ && IOS` branch -> RESAMPLER_QUALITY_LOWER=2) up to non-mobile RESAMPLER_QUALITY_NORMAL=3. Prior "2" was a drift-guard pin matching default — README §7 "audible quality gain" claim was inaccurate. NORMAL delivers actual SINC-quality bump over LOWER's linear interpolation at sub-1% A15 CPU cost. Verified via `libretro-common/include/audio/audio_resampler.h` enum + `config.def.h` v1.22.0.
- retroarch.cfg: + `input_max_users = "4"`. Was unset; v1.22.0 non-DINGUX default = 8 (`config.def.h@v1.22.0:1645`). tvOS RA hard-cap = 3 per #16685; Mupen pins pak1-4 = "rumble" for 4P parity. Value "4" matches Mupen 4P pak alignment + forward-compat headroom if cap raised; prevents phantom slots 5-8 in input config UI.
- retroarch.cfg: bump header stamp v3.24 -> v3.25; "73 keys" -> "74 keys".
- README.md: §7 Audio resampler row rewritten — "audible quality gain" narrative replaced with accurate "real flip from tvOS LOWER default to NORMAL; SINC vs linear; sub-1% A15 CPU cost". Value "2" -> "3".
- README.md: §7 Input section + new row for `input_max_users = "4"` (slot cap rationale + #16685 cross-reference + 4P alignment note).
- README.md: intro paragraph "73-key" -> "74-key".
- README.md: badge 3.24 -> 3.25.
- CHANGELOG.md: trim v3.20 entry per 5-release retention; retained entries are now v3.21-v3.25.
- Companion v3.25: 7 `.cfg` paired stamps v3.24 -> v3.25 (6 bodies byte-identical; Mupen64Plus-Next.cfg trims one verbose multi-clause audio comment line to single-line per editorial). 2 `.opt` files trimmed: `Genesis Plus GX.opt` 6 keys -> 3 keys (drift-guard pins `genesis_plus_gx_ym2612`, `genesis_plus_gx_audio_filter`, `genesis_plus_gx_render` removed — all match upstream Genesis-Plus-GX `libretro_core_options.h` defaults at L444/L475/L347 respectively); `Mupen64Plus-Next.opt` 10 keys -> 9 keys (inert `mupen64plus-43screensize = "320x240"` removed — under HAVE_THR_AL angrylion path, value is read at libretro/mupen64plus-libretro-nx `libretro/libretro.c:1348` then explicitly overwritten at L1370-1371 by `if(current_rdp_type == RDP_PLUGIN_ANGRYLION)` forced override to `retro_screen_width=640, retro_screen_height=480, retro_screen_aspect=4.0/3.0, AspectRatio=1`). 3 `.opt` files (Mesen / Mupen / mGBA) trim verbose multi-clause comments to single-line + drop legacy `(v3.X)` historical annotations per established v3.23 editorial rule (Beetle PCE Fast.opt / FinalBurn Neo.opt / Snes9x.opt unchanged — already minimal). 1 `.cfg` file (Mupen64Plus-Next.cfg) trims one verbose multi-clause audio comment line to single-line; remaining 6 `.cfg` bodies byte-identical to v3.24. README §1 Genesis row keys 6 -> 3; Mupen row keys 10 -> 9 (drift-guard rationales removed where keys removed; 43screensize reference removed). README badge 3.24 -> 3.25. CHANGELOG trim v3.20 per matching 5-release retention.
- cfg 22, opt 25 -> 21, cfg+opt 47 -> 43.

# 3.24 - 2026-04-25

- v3.24: §8 shader lineup narrowed to zfast-crt and lcd-grid-v2; 0 cfg key-value changes.
- retroarch.cfg: bump header stamp v3.23 -> v3.24; 73 keys byte-identical.
- README.md: §8 Recommended presets table replaced — 3 rows (`crt-easymode`, `crt-aperture`, `crt-geom`) -> 2 rows (`crt/zfast-crt.slangp` Minimal cost as sole CRT recommendation; `handheld/lcd-grid-v2.slangp` Minimal cost as sole mGBA-LCD recommendation). zfast-crt characteristics: single-pass, `scale_type = viewport`, integer-scale safe, designed for low-end GPUs. Filenames verified against upstream `libretro/slang-shaders` master.
- README.md: §8 "Handheld note" callout updates `crt-easymode` -> `zfast-crt` reference. "Applying ... per-core is a safe starting point" sentence retargets `crt-easymode.slangp` -> `zfast-crt.slangp`.
- README.md: §8 parameter callout retargeted — "crt-easymode 4K parameters" (SHARPNESS_IMAGE/EDGES, GLOW_*, MASK_COLORS/STRENGTH/SIZE, SCANLINE_*, GAMMA_*, BRIGHTNESS) -> "zfast-crt 4K parameters" (BLURSCALEX, LOWLUMSCAN, HILUMSCAN, BRIGHTBOOST, MASK_DARK, MASK_FADE). Parameter names verified against `crt/shaders/zfast_crt/zfast_crt_impl.inc` on libretro/slang-shaders master.
- README.md: §8 "Integer Scaling Conflict" callout dropped entirely — both recommended presets are single-pass + integer-scale safe; geometry-shader caveat is no longer relevant to the documented lineup. Multi-pass shaders that would conflict (CRT-Royale, CRT-Geom-Deluxe, Mega Bezel, etc.) remain covered by the surviving "Avoid on Apple TV" line.
- README.md: §8 "Applying a shader" step 3 cross-reference updated from "crt-easymode 4K parameters" to "zfast-crt 4K parameters".
- README.md: badge 3.23 -> 3.24.
- README.md: intro paragraph "77-key `retroarch.cfg`" -> "73-key `retroarch.cfg`". Drift since v3.19; key count moved 77 -> 74 (v3.19) -> 70 (v3.20) -> 73 (v3.21) and has held at 73 through v3.22-v3.24. Intro was missed in each of those passes.
- CHANGELOG.md: trim v3.19 entry per 5-release retention; retained entries are now v3.20-v3.24.
- Companion v3.24: 7 `.cfg` paired stamps v3.23 -> v3.24 (bodies byte-identical to v3.23). README §5 Shaders retargets recommended-starting-point reference from `crt-easymode.slangp` to `crt/zfast-crt.slangp`; concurrent fix to stale "8 per-core `.cfg` files" -> "7 per-core `.cfg` files" (v3.22 dropped PCSX-ReARMed; §1 supported cores table was updated then but §5 prose was missed). mGBA LCD recommendation (`handheld/lcd-grid-v2.slangp`) unchanged. CHANGELOG trim v3.19 per matching 5-release retention. cfg+opt 47 — unchanged.

# 3.23 - 2026-04-25

- v3.23: README de-versioning pass; 0 cfg key-value changes.
- retroarch.cfg: bump header stamp v3.22 -> v3.23; 73 keys byte-identical.
- README.md: §7 Additional settings preamble rewritten to drop v3.19/v3.20/v3.21 trim-and-restore history; replaced with terse one-paragraph summary of current state.
- README.md: §7 Additional settings table — 14 row Notes columns de-versioned (drop "v3.5"/"v3.8"/"v3.15"/"v3.21" annotations from Drivers/XMB Animations/XMB Shader Pipeline/XMB Color Theme/XMB Shadows/Core Info Cache/HDR/Auto Game Focus/Pause on Menu/Pause on Focus Loss/State Thumbnails/Audio Latency/Resampler Quality/Audio Sync rows).
- README.md: §7 Hotkeys callout drops "v3.3:" prefix on Tier 2 autosave_interval line.
- README.md: §7 Video table — Refresh Rate row drops "v3.10"; On-Screen FPS row drops "v3.12" + "removed as drift-guard in v3.9"; Run Ahead row drops "(v3.11 — companion no longer pins...)"; Run-Ahead Mode row drops "as of v3.11"; Fast Forward Ratio row drops "v3.6 standard cap (was 5.0 in v3.3-v3.5...)".
- README.md: §9 Mupen Tier 2 row de-versioned (drops v3.3/v3.9/v3.21 annotations on multithread/FrameDuping/pak1-4 + v3.10/v3.9 on audio_latency + v3.9 on audio_sync).
- README.md: §10 Known Issues #4 row drops "global true as of v2.66" + "refactored upstream v1.20.0".
- README.md: §13 Versioning drops historical re-alignment example "(e.g. v3.0 re-aligned the two repos after they had evolved independently at v2.95 / v1.57)".
- README.md: badge 3.22 -> 3.23.
- CHANGELOG.md: trim v3.18 entry per 5-release retention; retained entries are now v3.19-v3.23.
- Companion v3.23: 7 `.cfg` paired stamps v3.22 -> v3.23 (bodies byte-identical to v3.22). Mupen64Plus-Next.cfg header drops "(v3.3 stutter mitigations)" inline annotation. Mupen64Plus-Next.opt header drops "(v3.3 Angrylion-MT heterogeneous-ARM tune)" and "(v3.21 P2-P4 parity for [titles])"; pak2/3/4 inline comment drops "v3.21:" prefix. README §1 Genesis row drops "(v3.1 off; v3.2 enum fix)"; mGBA row drops "(v3.9 enum fix...)" + "(v3.2 enum fix...)"; Mupen row drops v3.21/v3.3/v3.9 annotations. README §4 Frontend Override Keys table — run_ahead_secondary_instance drops v3.11; audio_latency drops v3.10/v3.9/v3.6-v3.8; audio_sync drops v3.9/v3.3-v3.8; autosave_interval drops v3.3; video_frame_delay_auto drops v3.11; closing inherited-keys paragraph drops v3.11. README §11 Versioning drops the same v3.0/v2.95/v1.57 historical example. CHANGELOG trim v3.18 per matching 5-release retention. cfg+opt 47 — unchanged.
- Establishes editorial rule going forward: README narrative does not carry per-version history; CHANGELOG is the sole record of what changed when.

# 3.22 - 2026-04-25

- v3.22: PlayStation 1 / PCSX-ReARMed core retired; shader recommendations trimmed to primary set. 0 key-value changes in retroarch.cfg.
- retroarch.cfg: bump header stamp v3.21 -> v3.22; 73 keys byte-identical.
- README.md: §1 BIOS files row drops "PS1" from the requires-BIOS list.
- README.md: §4 Filesystem layout tree drops `psx/` ROMs subfolder.
- README.md: §5 ROM folder reference drops PlayStation 1 row.
- README.md: §5 BIOS files table drops PlayStation 1 row; prose drops `scph5501.bin` case-sensitivity example (no longer applicable).
- README.md: §7 Video table Integer Overscale row drops "PCSX (variable 256-640)" mention.
- README.md: §7 Audio table — Audio Latency row drops "PCSX mirrors `48` (drift-guard)" clause; Resampler Quality row drops "PS1 44100 Hz" example; Audio Sync row drops "PCSX inherits global" clause.
- README.md: §8 Recommended presets table trimmed from 9 shaders to 3 primary CRT shaders — `crt-easymode.slangp` (recommended starting point, single-pass + integer-scale safe), `crt-aperture.slangp` (aperture-grille classic), `crt-geom.slangp` (curvature classic). Drops Minimal-cost row entries (`zfast_crt`, `crt-pi`, `crt-potato-warm/cool`), additional Low-cost variants (`crt-hyllian`, `fakelottes`), and Medium `crt-lottes-fast`. "Apply crt-easymode" follow-up paragraph rewritten to drop Minimal-preset fallback list. Integer Scaling Conflict callout drops `crt-hyllian` from multi-pass examples and `zfast_crt` / `crt-pi` from simple-shader examples (only `crt-geom` / `crt-easymode` / `crt-aperture` remain referenced).
- README.md: §9 Supported Systems table drops Tier 2 PlayStation 1 row (Mupen64Plus-Next is now sole Tier 2 entry); Mupen row gains v3.21 `pak1/2/3/4 = "rumble"` mention.
- README.md: badge 3.21 -> 3.22.
- CHANGELOG.md: trim v3.17 entry per 5-release retention; retained entries are now v3.18-v3.22.
- Companion v3.22: cores 8 -> 7. `config/PCSX-ReARMed.cfg` deleted (-8 keys); `config/PCSX-ReARMed.opt` deleted (-6 keys); 7 surviving `.cfg` paired stamps v3.21 -> v3.22; README cores badge 8 -> 7; §1 supported cores table drops PCSX row (Mupen row updated for v3.21 pak1-4 rumble); §2 file structure tree drops 2 PCSX entries; §2 ZIP-ships line "8/8" -> "7/7"; §4 Frontend Override Keys table de-PCSX'd across 9 rows (`run_ahead_enabled`, `run_ahead_secondary_instance`, `audio_latency`, `audio_sync`, `autosave_interval`, `video_scale_integer` row dropped entirely as no longer applicable, `video_scale_integer_scaling`, `video_frame_delay_auto`, `rewind_enable`); §7 install tree drops PCSX-ReARMed/ block; §8 Overclocking drops `psxclock` paragraph entirely; CHANGELOG trim v3.17 per matching 5-release retention.
- cfg 30 -> 22, opt 31 -> 25, cfg+opt 61 -> 47.
