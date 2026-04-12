retroarch-appletv4k changelog

2026-04-11  Ryan Musante

	* Trim README to install, layout, supported systems, and global setting notes
	* Keep changelog in kernel.org style
	* Remove stale Cloud Sync README section and scope table
	* Set cloud_sync_enable to false and clear the cloud sync driver value
	* Narrow README wording for disabled network interfaces to match the shipped config
	* Restore concise README summary, filesystem notes, and saving guidance without cloud sync language
	* Document Mupen64Plus-Next as Angrylion RDP with cxd4 RSP from the companion override pack
	* Reduce content_favorites_size from 100 to 10 and content_history_size from 50 to 10
	* Sync README, changelog, and retroarch.cfg after removing path-based directory assignments from the shipped config
	* Remove shipped path assignments for browser, BIOS, save, and state directories and document them as manual UI settings
	* Make README the sole layout reference and document per-core override directories under config/

2026-04-09  Ryan Musante

	* Rename invalid savefile_file_compression key to save_file_compression
	* Rename invalid cloud_sync_backend key to cloud_sync_driver
	* Remove nonexistent recording_enable setting
	* Remove redundant menu_left_thumbnails key
	* Rewrite audio_rate_control_delta comment for refresh-drift scope
	* Rewrite preemptive frames comment to match the effective Tier 1 and Tier 2 layering
	* Sync README additional-settings table and version badge

2026-04-06  Ryan Musante

	* Expand overscale comment to list all affected cores
	* Correct changelog count for invalid keys fixed in the prior entry
	* Rename invalid audio_output_rate key to audio_out_rate and continue cfg cleanup
	* Note swapchain guidance for 120 Hz panels
	* Add compression and runahead keys and remove redundant tvOS defaults
	* Sync README video_threaded documentation with retroarch.cfg
	* Consolidate README sections while continuing cfg tuning for tvOS 26

2026-04-04  Ryan Musante

	* Move per-core overrides to the retroarch-configs repo
	* Add video_threaded, audio output rate, and save-state retention settings
	* Remove bundled config directory from this repo
	* Add per-core override configs for the earlier split repository stage
	* Correct video_refresh_rate to 59.940060
	* Apply fact-check corrections and add the MIT license
	* Add badges and restructure the README during the markdown conversion
	* Continue iterative cfg tuning for the 64 GB Wi-Fi model
	* Initial drop-in configuration release
