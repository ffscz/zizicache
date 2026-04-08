# ZiziCache – Changelog

All notable changes to ZiziCache are documented here.

## 1.0.1 – 2026-04-08
- **NEW:** Migration from Lemon Squeezy to the new payment system Creem.io (EU Based, from Estonia). All existing license keys and updates remain fully functional. New payment, registrations and licenses are now handled through Creem.io.

## 1.0.0 – 2026-03-30
- **NEW:** ZiziBlocks — ESI-like Dynamic Blocks system that enables full page caching while serving personalized, per-user content (cart count, mini-cart, prices, user greeting, nonces) via client-side hydration through a single batched REST API request.
- **NEW:** ZiziBlocks WooCommerce integration — dynamically hydrates cart count, mini-cart widget, product prices, and stock status on cached pages without invalidating the page cache.
- **NEW:** ZiziBlocks multi-currency support — detects active currency from cookies (WOOCS/FOX, WCML, Aelia, VillaTheme) and returns prices in the correct currency per user, eliminating the need for per-currency cache variants.
- **NEW:** ZiziBlocks User Blocks — dynamically renders user greeting, avatar, login/logout link and account menu for logged-in users on otherwise fully cached pages.
- **NEW:** ZiziBlocks Nonce Blocks — replaces WordPress nonce fields (`_wpnonce`, WooCommerce nonce, comment nonce) with always-fresh values, fixing broken forms on pages with long cache TTLs.
- **NEW:** ZiziBlocks fragment cache layer with Redis/Memcached/file fallback; per-block configurable TTL and cache invalidation hooks.
- **NEW:** ZiziBlocks IntersectionObserver lazy hydration — medium/low priority blocks (prices below the fold) are only fetched when they approach the viewport, reducing initial batch request size.
- **NEW:** ZiziBlocks CLS prevention — placeholders carry `min-width`, `min-height`, and `contain-intrinsic-size` attributes matching the original element dimensions, preventing layout shift during hydration.
- **NEW:** ZiziBlocks `<noscript>` fallback — original content is preserved inside `<noscript>` tags within every placeholder, ensuring correct display for crawlers and users without JavaScript.
- **NEW:** ZiziBlocks admin UI tab (Dynamic Blocks) with per-feature toggles, live status, cache stats, and registered block list.
- **FIXED:** ZiziBlocks batch API response keying — multiple blocks of the same type (e.g. `wc-price` for different products on a shop listing page) now return individually keyed results, preventing all products from displaying the same price.
- **FIXED:** ZiziBlocks REST route collision — `/blocks/status` and `/blocks/cache` endpoints are now registered before the wildcard `(?P<block_id>[a-zA-Z0-9_-]+)` route, ensuring status and cache-management endpoints resolve correctly.
- **FIXED:** ZiziBlocks fetch timeout was configured (`blocks_timeout`, default 5000 ms) but never enforced. Requests to `/blocks/batch` now use `AbortController` with the configured timeout, preventing indefinitely hanging hydration requests.
- **FIXED:** ZiziBlocks cache key generation switched from `serialize()` to `wp_json_encode()` for deterministic, injection-safe fragment cache keys.
- **FIXED:** FTP Image Auto-Registration no longer enters an infinite re-registration loop on sites using WordPress 5.3+ scaled images (e.g. `1-scaled.jpg`). The duplicate detection now checks both the original path and its `-scaled` variant against a single batch-loaded lookup, replacing N per-file SQL queries with one.
- **FIXED:** `wp_generate_attachment_metadata()` replaced with a lightweight `getimagesize()` approach in `FTPImageRegistration`, preventing out-of-memory crashes on very large images (100+ MP). Memory-safety guard retained for edge cases.
- **FIXED:** Removed duplicate `_wp_attached_file` meta row that was being created by both `wp_insert_attachment()` and an extra `add_post_meta()` call.
- **FIXED:** Thumbnail and derivative file filter in `FTPImageRegistration::scan_directory()` now applies to all image extensions, not only WebP/AVIF.
- **FIXED:** `MediaLibraryIndexer::is_file_indexed()` now correctly recognises an original image as already indexed when WordPress stored it under its `-scaled` variant name, preventing repeated false-positive indexing attempts.
- **FIXED:** `MediaLibraryIndexer::get_unindexed_stats()` no longer counts WordPress derivative files (thumbnails, `-scaled`, `-rotated`, double-extension converted WebP/AVIF) as unindexed originals, eliminating spurious full-scan triggers.
- **FIXED:** `MediaLibraryIndexer::index_file()` duplicate check replaced with a direct `$wpdb->get_var` query (including `-scaled` variant lookup) instead of the heavyweight `get_posts()` + `meta_query` JOIN.
- **FIXED:** Regex pattern for stripping `<script>`, `<noscript>`, and `<template>` tags now uses possessive quantifiers to prevent catastrophic backtracking on malformed HTML.
- **FIXED:** HTML attribute parser (`get_atts_array()`) now correctly handles edge-case attributes using a branch-reset regex pattern.
- **FIXED:** Disabled RSS feeds now return HTTP 410 (Gone) instead of a 301 redirect, correctly signaling permanent removal to search engines.
- **IMPROVED:** `MediaLibraryIndexer` now skips derivative files early in `scan_and_index_uploads()` before any cache lookups, reducing iteration overhead on large uploads directories.
- **IMPROVED:** `MediaLibraryIndexer::maybe_auto_index_uploads()` now exits immediately if the FTP scan transient is active, preventing a redundant double-scan on every Media Library page load.
- **IMPROVED:** CSS parser engine upgraded to Sabberworm PHP CSS Parser 9.3.0, adding native support for CSS `@layer`, `@scope`, `@starting-style`, container queries, escaped quotes in selectors, improved `calc()` parsing, and PHP 8.5 compatibility.
- **IMPROVED:** All vendor libraries (Sabberworm CSS Parser, Safe polyfill) are now namespace-isolated under `ZiziCache\Vendor\*` to prevent conflicts with other plugins shipping the same libraries.
- **IMPROVED:** CSS `@layer` forward declarations are now properly normalized and re-serialized in both RUCSS and Critical CSS pipelines.
- **IMPROVED:** CDN URL rewriting now applies to inline `style` attributes in addition to `<link>` and `<style>` tags.
- **IMPROVED:** WooCommerce unused CSS now differentiates product types (`product-variable`, `product-grouped`, `product-external`) for more accurate per-page used CSS generation.

## 0.9.9-rc – 2026-03-15
- **FIXED:** Custom CSS selectors (configured in JavaScript settings) now correctly apply to both lazy render methods – CSS-only and Hybrid. Previously, custom selectors were only reliably processed in Hybrid mode.
- **FIXED:** "Exclude from lazy render" selectors now correctly suppress lazy rendering in both CSS-only and Hybrid methods. The selector detection and exclusion check now run before the method switch, ensuring identical behavior across both modes.
- **IMPROVED:** Lazy render pipeline now merges auto-detected, custom, and `.lazy-render` selectors in a single pass shared by both methods, eliminating any risk of divergence between CSS-only and Hybrid output.

## 0.9.8-rc – 2026-03-13
- **FIXED:** Page cache preload now starts automatically immediately after saving settings (mode switch, cache enable, or preload enable). Previously, preload only triggered via WP-Cron after the full cache lifespan interval, leaving the cache empty for hours after a fresh install or mode change.
- **FIXED:** File-based mode now correctly generates `preloader-list.txt` on first save, ensuring cache warming works reliably from the start.
- **FIXED:** First-run notice on the Overview dashboard now correctly informs the user to save Page Cache settings to activate preload, instead of misleadingly suggesting the cache is already warming.
- **FIXED:** PreloadSqlite activation check now correctly verifies the SQLite extension availability, preventing a silent failure on servers without the `sqlite3` PHP extension. (Thanks to Josef Helie)
- **IMPROVED:** LiteSpeed mode preference flags (`cache_preload`, `cache_separate_mobile`, `cache_404`) are now preserved when switching from LiteSpeed to File-based mode instead of being reset to false.

## 0.9.7-rc – 2026-03-09
- **IMPROVED:** Improved page caching on OLS.
- **IMPROVED:** Enhanced URL handling for converted images.
- **IMPROVED:** Updated logic for detecting background images.

## 0.9.6-rc – 2026-03-09
- **IMPROVED:** Improved detection of converted images to AVIF and WebP, with fallback to the original (content negotiation).

## 0.9.5-rc – 2026-03-05
- **BUGFIX:** Fixed the preload and purge cache logic on LiteSpeed servers.
- **IMPROVED:** `?nocache=1` or any arbitrary parameter (e.g., `?unknown_param=xy`) now works on LiteSpeed servers to bypass the cache.
- **IMPROVED:** Unified logic for cache_lifespan and scheduled caches across the entire plugin in LS mode and File Based Page Cache Mode.
- **IMPROVED:** Auto mode has been removed from Page Cache Settings > Cache Mode, leaving only LiteSpeed Cache (preselected on LiteSpeed servers) and File-based Cache with automatic detection, while non-LiteSpeed servers will always have only File-based Page Cache available.

## 0.9.4-rc – 2026-03-04
- **FIXED:** LCP HMAC validation now supports cache-lifespan-aware verification windows, improving reliability on long-lived cached pages.
- **FIXED:** Added safe backward compatibility for pre-update hourly HMAC tokens during transition after plugin update.
- **FIXED:** Page cache is now purged automatically on plugin upgrade to regenerate pages with fresh security tokens.
- **FIXED:** Database Monitor warning (`Undefined variable $prev_total`) resolved; database size change history is now calculated correctly.
- **FIXED:** Redis status diagnostic success/error logs are now debug-only to reduce production log noise.
- **FIXED:** Varnish integration hardened against undefined `$http_response_header` warnings when test requests fail.
- **FIXED:** zizi-log.log adjustments after transition from beta – minimization of non-critical function notices, reduction of log spam, and decreased I/O operations.
- **IMPROVED:** Stability and migration safety for large production websites during incremental updates.
- **IMPROVED:** Licence validation.
- **IMPROVED:** preload_async_enabled for faster and more efficient asynchronous purge and preload operations of updated page cache content. Reduction of background processes resulted in a relative latency decrease of approximately 93.52%.

## 0.9.3-rc – 2026-03-02
- **FIXED:** API update fix.
- **FIXED:** Better WooCommerce handling.

## 0.9.2-rc – 2026-03-01
- **IMPROVED:** Bulk Optimization in the Image Converter section. More accurate calculation of images to be converted.
- **IMPROVED:** Adjustments for smoother and faster bulk image conversion.
- **FIXED:** Adjustments hybrid mode in Lazy render page elements.

## 0.9.1-rc – 2026-02-27
- **FIXED:** Font preload logic now correctly detects ATF fonts as LCP for proper early preloading.
- **FIXED:** Non-cached page counting has been adjusted for more accurate cache statistics.
- **FIXED:** Image conversion logic optimized for more stable and faster processing.
- **FIXED:** Multiple UI improvements for a smoother and more consistent user experience.
- **FIXED:** Plugin detection enhanced for more precise and targeted page cache purge, especially when saving content via builders or WPML.
- **FIXED:** Better handler Redis Object Cache and Memcached with flush OC during ZiziCache plugin deactivation.

## 0.9.0-beta.rc – 2026-02-24 (post-audit hardening)
- **FIXED:** LiteSpeed preload telemetry now maps cache results correctly into Cache URL Tracker (consistent `hit|miss|nocache|unknown|error` status model).
- **FIXED:** Fragment block cache invalidation now reliably targets entries by `block_id` metadata to prevent stale dynamic blocks.
- **SECURITY:** Public `/blocks/batch` endpoint hardened with request body size limit, max blocks per batch, and IP-based rate limiting.
- **IMPROVED:** Legacy RUCSS fallback now requires explicit opt-in (`css_allow_legacy_rucss_fallback`) for safer default behavior.
- **IMPROVED:** CSS minification file-size guard is now filterable via `zizi_cache_css_minify_max_bytes`.

## 0.8.9-beta.rc – 2026-02-19
- **IMPROVED:** Improved UI.
- **IMPROVED:** Better handling of page cache on NGINX and Apache servers. Enhanced limit for the page cache preload worker.

## 0.8.8-beta.rc – 2026-02-12
- **IMPROVED:** Image Optimization (Optimization → Image Optimization) now provides more accurate above-the-fold image detection for both mobile and desktop, including preload support and the ability to define the preload order. Note: an excessive number of preloaded images may unnecessarily block bandwidth.
- **IMPROVED:** Font Optimizer with more accurate font detection and improved compatibility with the Bricks builder.

## 0.8.7-beta.rc – 2026-02-09
- **IMPROVED:** Optimization of the plugin has been implemented on both the frontend and backend. Modules are now loaded only where they are actually needed.
- **FIX:** A bug in Remove Redundancy during settings saving has been fixed.

## 0.8.6-beta.rc – 2026-02-02
- **IMPROVED:** Added support for both Memcache and Memcached. Uses the modern PECL memcached extension (with "d") alongside legacy memcache. This allows better performance, SASL authentication.

## 0.8.5-beta.rc – 2026-01-29
- **FIXED:** Improved Memcached Object Cache UI: checks for server availability and required PHP extensions before activation. Mutual exclusion updated to prevent conflicts when Redis or Memcached is active during object-cache.php regeneration.

## 0.8.4-beta.rc – 2026-01-28
- **NEW:** Added option to use Memcache. If Redis Object Cache is not available, Memcached can be enabled if available.
- **IMPROVED:** Bulk Optimizer in Image Converter now shows progress correctly in time intervals after several seconds.
- **IMPROVED:** In BFCache, merged URL Exclusions section with BFCache Configuration.
- **FIXED:** Adjusted logic for more precise detection of Cart and Checkout for their exclusion from page cache and BFCache.
- **FIXED:** Adjusted method for purging page cache when using ACF.
- **FIXED:** Adjustment of bypass rules for optimization for logged-in administrators.

## 0.8.3-beta.rc – 2026-01-14
- **IMPROVED:** In the CSS Optimization section, the optimization type is now correctly labeled as CSS Loading Method.
- **IMPROVED:** In the Critical CSS Generation section, a new option allows choosing between Inline CSS and File-based delivery. Each method has its own advantages and disadvantages. Critical CSS accuracy ranges between 30–70%.
- **IMPROVED:** In the Image Optimization section, the logic for Priority Hints has been refined.
- **IMPROVED:** In the Font Optimization section, added the ability to define custom URLs for font preload. Note that this applies only to locally hosted fonts.
- **IMPROVED:** In the Image Converter section, the logic for converting images to AVIF and WebP has been refined.
- **IMPROVED:** In the Object Cache section, added support for connecting to Redis using ACL authentication (username + password), allowing the use of Redis with ACL (thanks for the suggestion, @Vincent Chan).
- **FIXED:** In the Image Optimization section, the exclusion logic for lazy loading has been corrected for both desktop and mobile versions.
- **FIXED:** Reworked Font Optimization logic for collecting critical fonts to ensure correct preloading.
- **FIXED:** Corrected the loading order of ZiziCache scripts used for collecting CWV metrics.
- **FIXED:** Removed unnecessary autoload entries in the wp_options table during font statistics collection (thanks for the suggestion, @Jakub Schnicel Rezníček).
- **ADDED:** In the Remove Redundancy section, 14 new features have been added with the ability to disable them individually.
- **EXPERIMENTAL:** Added an option to preload jquery.min.js in Optimization > JavaScript Optimization. This may be useful but it occupies an HTTP/2 connection slot.

## 0.8.2-beta.rc – 2026-01-09
- **FIXED:** Adjusted autoload values used by the plugin, which previously overfilled the wp_options table. Only essential items are now autoloaded.
- **FIXED:** Compatibility issue on some hosting environments where mod_access_compat was not enabled.
- **IMPROVED:** Complete overhaul of the CSS optimization section, specifically delivery methods (On Interaction, DOMContentLoaded Event, and Async).
- **IMPROVED:** Critical CSS delivery method now allows choosing between inline insertion or file-based insertion for overall reduction. This is an optimization improvement, not a precise per-page Critical CSS generator. Accuracy is approximate, but performance gains are significant.
- **FIXED:** Exclude and preload functionality for above-the-fold images corrected for both mobile and desktop versions.
- **BUGFIX:** Purge All now correctly removes all CSS.
- **BUGFIX:** Migration from older plugin versions could break the Font Detector; logic has been fixed.
- **FIXED:** Dependency chain for LCP, TTFB, and Metrics files corrected.

## 0.8.1-beta.rc – 2025-12-29
- **IMPROVED:** Better detection of Cloudflare outages for Cloudflare Failover.
- **NEW:** Added support for native page cache on LiteSpeed servers. Three modes are now available: Auto, LiteSpeed Cache Only, and File-based Cache Only (suitable not only for Apache servers but, of course, also for LiteSpeed servers).
- **NEW:** Added a page to overview URLs missing from the page cache, available under Page Cache > Pages Not Cached.
- **IMPROVED:** Minor adjustments in the admin UI.

## 0.8.0-beta.rc – 2025-12-16
- **IMPROVED:** Enhanced Redis Object Cache connectors.
- **IMPROVED:** Updated BFCache logic.
- **IMPROVED:** Improved UI in the Images section.
- **FIXED:** Fixed Page Cache generation logic, including support for mobile versions.
- **FIXED:** Fixed recalculation and detection of thumbnail dimensions.
- **FIXED:** Fixed logic for CSS Loading Methods.
- **FIXED:** Minor adjustments in the admin UI.
- **NEW:** Added the ability to view pages that are not in the Page Cache. You can now see the exact reason why pages are excluded from caching or not cached, either due to internal plugin rules and bypasses or other settings.

## 0.7.9-beta.rc – 2025-12-11
- **IMPROVED:** Enhanced UI in the Redis Object Cache section with a more reliable handler for enabling and disabling Redis Object Cache.
- **IMPROVED:** Updated logic for writing constants to wp-config.php.
- **IMPROVED:** Improved UI in the 103 Early Hints section with added explanations about how and where it works.

## 0.7.8-beta.rc – 2025-12-10
- **FIX:** Resolved a potential crash in Image Converter diagnostics by replacing wp_tempnam() with tempnam() and removing a duplicate admin_init hook. Image conversion functionality remains unchanged.
- **FIX:** Ensure page-cache enable/disable toggles correctly and dependent optimizations respect cache state and configuration dependencies.
- **FIX:** Fix Redis connection handling and Unix-socket support (on OpenLiteSpeed servers) — avoid attempting connections with an empty host (php_network_getaddresses error), correctly detect and use Unix socket paths, and prevent undefined "host"/"port" errors in the generated object-cache.php.
- **FIX:** Reduce verbose debug logging and stop spurious DB/JSON errors by adding Action Scheduler table-existence checks.
- **FIX:** Suppressing noisy BFCache/Image/Font/SysConfig debug output, replacing direct error_log() calls with CacheSys::writeError().
- **FIX:** Preventing BFCache status/REST checks when the feature is disabled.

## 0.7.7-beta.rc – 2025-12-08
- **IMPROVED:** Added a global option to enable CWV metric collection directly from the admin interface, in addition to WP-CLI and direct DB activation.
- **IMPROVED:** Added bypass URL parameters: `?nocache=1`, `?bypass_optimization=1`, and `?zizi_bypass=1` (e.g., `https://example.com/?nocache=1`).

## 0.7.6-beta.rc – 2025-12-08
- **FIX:** Fixed an issue with the toast notification when saving settings in Optimization > IFrame & Video Optimization. (Thanks to @schnicel)

## 0.7.5-beta.rc – 2025-12-08
- **NEW: Modular Admin Interface** - Complete redesign of admin panel architecture: Separated components for each settings section, dedicated REST API handlers for each module, admin templates reorganized into standalone files, Highcharts integration for performance visualizations, toast notification system for user feedback, toggle UI components for consistent switch controls.
- **NEW: BFCache (Back/Forward Cache) Management** - Complete browser cache optimization system for instant back/forward navigation: Session token management for logged-in users with automatic HTTP header modification, cookie-based authentication state tracking for cache invalidation, support for WordPress Script Modules API (WordPress 6.5+) with legacy fallback, automatic exclusion for WooCommerce admin pages to prevent JavaScript conflicts, diagnostics and status reporting via REST API.
- **NEW: Core Web Vitals (CWV) Monitoring System** - Real-time performance tracking: MySQL database layer for high-performance CWV data storage with optimized schema, LoAF (Long Animation Frame) detector with Layout Thrashing analysis and severity classification, TTFB monitoring with threshold alerts, LCP element details extraction for debugging optimization opportunities, frontend banner for administrators showing real-time CWV status per page, REST API endpoints for metrics collection, page summaries, and data management, configurable sampling rates, thresholds, and retention policies, automatic aggregation via WP-Cron with 5-minute intervals.
- **NEW: Cloudflare CDN Integration** - Native Cloudflare cache management: API Token and Zone ID configuration with connection testing, automatic cache purge on content changes, smart URL collection for targeted purge instead of full zone purge, Edge Cache Rules management, conflict detection with official Cloudflare plugin and WP Rocket, Emergency Failover (EXPERIMENTAL feature).
- **NEW: Font Auto-Optimizer** - Automatic Google Fonts CSS generation: WP-Cron scheduled optimization respecting page cache lifespan, aggregated font statistics from collected data, automatic WOFF2 URL extraction for preloading, minimum data threshold before auto-generation (10 pages).
- **NEW: Image Converter Metadata Validator** - Self-healing metadata system: Automatic validation when image format configuration changes, sanity check on post save for _wp_attached_file, background WP-Cron validation for non-blocking operation, fixes GUID, attached_file, and optimization metadata.
- **NEW: Cache Statistics Monitor** - Hit/miss tracking: Hourly transient-based statistics for responsiveness, persistent stats with sampling to reduce database load, real-time hit rate calculation.
- **NEW: Cache Settings REST API** - Dedicated endpoints for cache configuration: `/cache-settings` GET/POST for cache lifespan, preload, mobile cache settings; `/cache-exclusions` GET/POST for bypass URLs, cookies, query string management; automatic WP_CACHE constant management in wp-config.php; integration with unified SysConfig system.
- **IMPROVED:** Authority class with new `isAllowedSilent()` method for permission checks without logging.
- **IMPROVED:** Redundancy module - Enhanced jQuery Migrate disabling with proper deregistration and JS Delay exclusion.
- **IMPROVED:** Redundancy module - Added WooCommerce tracking scripts to disable list (sourcebuster-js, wc-order-attribution).
- **IMPROVED:** Redundancy module - Enhanced `hide_wp_version()` with all generator filter types including Elementor.
- **IMPROVED:** REST API disable now allows ZiziCache LCP and Font Intelligence endpoints for anonymous users.
- **IMPROVED:** Bootstrap sequence - Earlier initialization of Redundancy module for wp_default_scripts hook.
- **IMPROVED:** WooCommerce admin bypass detection moved earlier in bootstrap to prevent BFCache conflicts.
- **IMPROVED:** Plugin activation now creates CWV MySQL tables alongside ImageConverter stats table.
- **IMPROVED:** Self-healing CWV table check on admin_init for seamless upgrades from older versions.
- **IMPROVED:** Frontend JavaScript reorganized - new modular bfcache-handler-module.js, bfcache-login-detector.js.
- **IMPROVED:** Quicklink integration fixed with new quicklink-integration-fixed.js replacing unstable version.
- **IMPROVED:** Admin CSS streamlined - removed legacy files in favor of modular approach.
- **FIXED:** Block library CSS now properly deregistered (not just dequeued) to prevent re-enqueueing.
- **SECURITY:** Enhanced REST API permission callback with CSRF protection and nonce verification.
- **SECURITY:** Detailed logging for denied access attempts with caller context.
- **COMPATIBILITY:** Tested with WordPress 6.9.

## 0.7.1-beta.7 – 2025-10-28
- **FIXED:** API Updater call.

## 0.7.0-beta.7 – 2025-10-27
- **IMPROVED:** Complete redesign of the admin interface for a more modern and user-friendly experience.
- **IMPROVED:** Major update to the Page Cache module — improved file counting accuracy and internal structure.
- **IMPROVED + FIXED:** Fully refactored Image Converter logic — redesigned detection of available images and thumbnails, rewritten WebP and AVIF conversion system with reliable fallback to original images. Resolved compatibility issues with various plugins and themes.
- **IMPROVED:** Comprehensive overhaul of the Font Optimization logic for better performance and reliability.
- **IMPROVED:** Full rework of the Optimization section, simplifying structure and improving stability.
- **FIXED:** Resolved compatibility issue with SEOPress when generating XML sitemaps.

## 0.5.7-beta.5
- **IMPROVED:** Added a button in the Optimization tab for saving settings in the Early Hints and Enhanced Navigation Strategies sections.
- **FIX:** Fixed a bug related to handling JavaScript in delay mode.

## 0.5.6-beta.5
- **ENHANCED:** Adjusted API calls for plugin license validation, minimizing requests to a single call to reduce database queries and external API endpoint load.

## 0.5.5-beta.5
- **ENHANCED:** Page cache preloader improved with advanced handling of expired cache – if expired, old cache is still served while URL is queued in SQLite preload.sqlite, worker calls URL with ?zizi_preload=1&zizi_warmed=1, OptiCore adds warmed footprint, new cache is stored with warmed marker, ensuring end-to-end preload workflow and warmed cache identification.

## 0.5.4-beta.5
- **UPDATE:** Completely reworked image handling — full rewrite of the image conversion workflow and automatic registration of converted images into the Media Library when images are uploaded manually via FTP.
- **FIX:** Fixed logic in the Redis Object Cache integration when the plugin is deactivated and re-activated.
- **FIX:** Improved compatibility with WooCommerce.

## 0.5.3-beta.5
- **FIX:** Improved Redis connection handling for Unix domain sockets.
- **FIX:** Image Converter: improved conversion progress indicators and optimized image statistics.
- **FIX:** Improved compatibility with popular page builders.
- **FIX:** Removed visual LCP detector when working inside page builders in the admin.

## 0.5.2-beta.5
- **NEW:** HTTP 103 Early Hints support - Preload critical resources before HTML parsing for faster page loads with server-side resource hints.
- **NEW:** Enhanced Navigation Strategies - Advanced Quicklink integration with intelligent prefetching and navigation optimization.
- **NEW:** Theme compatibility improvements - Added support for Bricks Builder, Elementor, and popular theme frameworks.
- **ENHANCEMENT:** Complete Image Converter statistics overhaul - Now uses filesystem scan as authoritative source for accurate WebP/AVIF counts.
- **ENHANCEMENT:** Improved JavaScript core with better lifecycle management, font loading optimization, and enhanced lazy loading.
- **ENHANCEMENT:** Vue.js admin interface improvements with consistent styling, loading indicators, and better user experience.
- **FIX:** Redis object cache configuration - Resolved Unix socket detection and connection issues.
- **FIX:** Iframe lazy loading optimization - Fixed YouTube/Vimeo embed loading and performance issues.
- **FIX:** Image Converter statistics accuracy - Fixed incorrect format counts that showed inverse of actual filesystem state.
- **FIX:** Database cleanup for stale conversion records - Automatic removal of orphaned statistics entries.

## 0.5.1-beta.5
- **FIX:** Fixed the Defer JavaScript Execution feature, where it was not possible to exclude specific JavaScript files according to user preferences.

## 0.5.0-beta.5
- **NEW:** Advanced Image Converter with AVIF-first approach - Automatically converts uploaded images to modern AVIF and WebP formats with up to 97% size reduction. Features intelligent fallback system, bulk processing, and comprehensive diagnostics.
- **NEW:** Added support for automatic deletion of original images after conversion, including async processing, safety checks (WooCommerce, featured, content), REST API endpoint, and bulk cleanup UI. Default behavior remains safe with originals preserved unless explicitly disabled.
- **NEW:** Plugin now auto-registers images used in WooCommerce; associated images are deleted when the product is removed.
- **ENHANCEMENT:** Complete compatibility overhaul that resolves JavaScript conflicts preventing post/page saves. Features advanced lazy rendering, block-level cache management, and performance optimizations without editor interference.
- **ENHANCEMENT:** Improved plugin admin UI, adjustment of input state after settings update.
- **FIX:** Resolved critical Gutenberg editor compatibility issues that caused "Unable to save changes" errors when editing posts or pages.
- **FIX:** Eliminated JavaScript bundle duplicates that were inflating plugin size and causing potential memory issues.
- **FIX:** Optimized webpack configuration to prevent development files from appearing in production builds.
- **FIX:** JavaScript Bundle Optimization - Implemented conditional loading system that reduces initial admin page load by up to 573KB. ApexCharts and heavy components now load only when needed.

## 0.4.8-beta.4
- **FIX:** Fixed API endpoint in Fonts > Font Statistics > Generate CSS.

## 0.4.7-beta.4
- **FIX:** GZIP compression and headers unified according to the working version, always only .gz files and universal gzip header.

## 0.4.6-beta.4
- **FIX:** Fixed WARMED cache detection logic in preloader system for improved accuracy.
- **FIX:** Corrected cache lifespan conversion from hours to seconds in advanced-cache.php template.
- **FIX:** Enhanced mobile cache support with proper user-agent detection and -mobile suffix variants.
- **NEW:** Comprehensive OpenLiteSpeed (OLS) server compatibility implementation.
- **NEW:** Advanced OLS server detection using LSWS_EDITION, SERVER_SOFTWARE, and environment variables.
- **NEW:** OpenLiteSpeed-optimized headers for LSAPI compatibility and modgzip integration.
- **NEW:** Enterprise vs OpenLiteSpeed differentiation with appropriate cache headers.
- **NEW:** Local changelog integration for plugin details popup from readme.txt.
- **NEW:** "View Changelog" and "Changelog" links in WordPress plugins page for licensed users.
- **ENHANCEMENT:** Simplified and more reliable WARMED cache footprint detection using fopen/fread.
- **ENHANCEMENT:** Improved serve-stale-while-revalidate functionality with SQLite queue processing.
- **ENHANCEMENT:** Enhanced universal fallback support for non-OLS servers (Apache, Nginx).
- Other: Performance optimizations, comprehensive testing, and technical documentation improvements.

## 0.4.1-beta.4
- **FIX:** Fixed potential XSS vulnerability in LCP detector (thanks to Vladimír Smitka).
- **FIX:** Fixed visibility of server-info API endpoint (thanks to Vladimír Smitka).
- **REFACTOR:** Complete overhaul of Font section and entire font optimization workflow.
- **REFACTOR:** Complete rework of CSS processing using Sabberworm PHP CSS Parser.
- **NEW:** Improved UI in Optimization section with individual save buttons for each category.
- Other: Minor UI improvements, performance enhancements and security fixes.

## 0.4.0-beta.4
- Added to the regular beta release update channel.
- Fix API key from the plugin updater - api.zizicache.com proxy (security).
- Fixed insertion of HTML comments and scripts into JSON responses (does not break API).
- Fixed PHP warnings for undefined config keys.
- Raised minimum required WordPress version to 6.1.
- Other minor security and stability improvements.

## 0.3.8-alpha.3
- **License Display:** Improved license information layout with compact vertical design.
- **License Management:** Enhanced activation status text with clear usage indicators.
- **Auto-Updater:** Fixed critical compatibility issues with LemonSqueezy updater system.
- **Diagnostics:** Added updater status endpoint and diagnostic tools.
- **UI/UX:** Better visual feedback for license activation limits and remaining slots.

## 0.3.7-alpha.3
- **Font Intelligence:** Enhanced client-side font detection system for improved accuracy.
- **Font Intelligence:** Added preload generation for critical fonts.
- **Font Intelligence:** Admin interface with font usage statistics.
- **Security Enhancement:** Added comprehensive security checks to uninstall.php.
- **Security:** Added audit logging, path validation, and file type restrictions.
- **Stability:** Improved error handling during plugin uninstall process.

## 0.3.7-alpha.2
- **License Management:** Implemented Lemon Squeezy API integration for premium features.
- **UI Enhancement:** New License management tab with activation/deactivation.
- **Auto-Updater:** Secure premium plugin delivery system.
- **Configuration:** Fixed cache_mobile configuration missing key issue.
