# ZiziCache – Changelog

All notable changes to ZiziCache are documented here.

== Changelog ==

## 1.3.3
- **IMPROVED:** Lazy load iframes – works with cookie consent plugins (Complianz, iubenda, Real Cookie Banner, Cookiebot, OneTrust, Usercentrics and others): an embed held back until consent is left to the consent plugin, so videos and maps show after consent instead of an empty frame.*
- **IMPROVED:** YouTube and Vimeo click-to-play and the Elementor video widget – a video held back by a consent plugin is left to the consent plugin and its placeholder; the player styles are added only to pages with a click-to-play box.*
- **IMPROVED:** YouTube and Vimeo click-to-play – sharp thumbnail also for a video held back by a consent plugin: after consent the player shows the sharp picture and the video loads on a click; Automatic quality is sharp on every device and a portrait video gets its portrait frame.*
- **IMPROVED:** Used CSS – pieces no page has linked for twice the cache lifespan are deleted by the lifespan job, so they no longer pile up in the cache folder.*
- **IMPROVED:** Nginx Helper – after a save, pages still in the cache are not built again (the homepage was rebuilt after every save).*

## 1.3.2
- **FIX:** Used CSS – keeps the cascade order: rules of stylesheets after a render-blocking style the Used CSS does not hold are printed after that style, so theme rules no longer lose to it and the page no longer shifts (CLS) when the original stylesheets load.*
- **FIX:** Overview – the hit rate counts visitors only: preload and bots are no longer counted as misses (file mode too); a note says when the Cloudflare edge serves pages the server never sees.*
- **IMPROVED:** Preload after saves – the homepage (and the homepage of every language) is preloaded at most once a minute instead of once per 10 minutes, so frequent product saves no longer leave it uncached; shop and category pages keep their 10-minute window, and a page purged inside its window is preloaded once when the window ends.*

## 1.3.1
- **FIX:** Cache 404 pages – old addresses of renamed posts and products and ?p=ID redirect (301) again; the cached 404 page is sent after the redirects of WordPress and other plugins.*
- **FIX:** Cache 404 pages – with NagataPress every language has its own 404 page.*
- **FIX:** Images – no second preload of the LCP image when the theme or another plugin already preloads it.*
- **FIX:** CSS – the FiboSearch stylesheet is no longer deferred (its layout is set by JavaScript, the page shifted).*
- **IMPROVED:** Used CSS – a shared Used CSS (builder mode) is rebuilt when a page needs rules it lacks, keeping the rules of the earlier pages.*

## 1.3.0 – 2026-11-09 =
- **FIX:** Cloudflare failover – only the site's own DNS records are switched, they are restored after three good checks, and also on Disconnect and on deactivation.*
- **NEW:** Cloudflare – the ZiziCache edge rule carries its own reference; a rule created by 1.2.6 is found and updated in place, no duplicate.*
- **FIX:** Multisite – every site of a network has its own page cache settings; a site without them is not served from the cache instead of using the settings of the main site.*
- **FIX:** Multisite – deactivating ZiziCache or turning off page cache on one site no longer removes the drop-in of the whole network, and a purge of one site keeps the cache of the others.*
- **FIX:** Multisite – Redis and Memcached are managed on the main site only: a save on another site rewrote the object cache of the whole network.*
- **FIX:** Multisite – the shared .htaccess uses the paths of the network (a subdirectory network ended in a redirect loop).*
- **FIX:** LiteSpeed mode – activating a language or currency plugin updates Cache-Vary in .htaccess, and a new network site no longer purges the whole server.*
- **FIX:** Page cache – the drop-in stays in sync after language plugin changes, page builder editor links are not stored, and the cache lifespan deletes only expired files.*
- **FIX:** Page cache – content edited while ZiziCache was inactive is purged when it is activated again.*
- **FIX:** Page cache – ?nocache and the bypass parameters skip the cache in the drop-in too.*
- **FIX:** Updater – a failed download no longer deletes the plugin folder; WordPress keeps its own temporary backup until the new version is installed.*
- **FIX:** Images – the size cleanup keeps files still used after deleted originals and removes only disabled sizes; WP-CLI deletes originals only with --delete-originals.*
- **FIX:** Images – the original cleanup continues where it stopped, front-end uploads get sizes and conversion, and the FTP import names skipped files.*
- **FIX:** Images – the delivery check no longer runs inside WP-CLI; "wp zizi-cache images delivery-test" schedules it for the next visit.*
- **FIX:** Server rules – the Nginx snippet uses the content URL, gives CSS, JS, SVG and fonts in the cache folder a year of browser cache, and denies SQLite side files and the settings folder of a network.*
- **FIX:** CSS – Minify CSS exclusions work, also with part of a file name; stylesheets without Used CSS keep blocking in Async mode.*
- **FIX:** Fonts – subsets, preload override and display strategy are applied as set.*
- **FIX:** JavaScript – exclusions added by integrations reach Delay JavaScript; the prerender guard defers the Meta Pixel instead of turning analytics off.*
- **FIX:** Purge – WooCommerce variations, Varnish, Cloudways, Nginx Helper (also from WP-CLI and a system cron) and Cloudflare APO purge what changed.*
- **FIX:** CDN – Pull CDN serves cached stylesheets with their url() rewritten.*
- **FIX:** Security – REST writes check the nonce for every writing method, and rate limits are atomic.*
- **FIX:** Self-hosted third-party files – the exact host is matched, every redirect is checked, polyfill.io is dropped.*
- **FIX:** BFCache – Clear All Tokens works.*
- **FIX:** Embeds – the YouTube box keeps the player ratio, Vimeo thumbnails come from oEmbed.*
- **NEW:** Logs – every entry has a level, and the admin has a log level select.*
- **NEW:** Redis and Memcached – a Remove button for stored usernames and passwords.*
- **IMPROVED:** Doctor and self test are translatable and use the names of the admin.*
- **IMPROVED:** Admin texts describe what the settings really do.*
- **CHANGED:** Redis – the hit rate is labelled as the figure of the whole Redis server since it started.*
- **FIX:** Cache for logged-in users – a page a plugin marks as private (membership, restricted content) is no longer cached.*

## 1.2.6
- **FIX:** Images – hidden images (a mega menu, closed panels) no longer count as above the fold: they keep lazy load, phones stop downloading them. Existing scan data is ignored and collected again.*
- **FIX:** Image Converter – the overview no longer counts images without any copy as converted, and explains images against files.*
- **FIX:** Image Converter – the run summary names the reason "larger", and the savings no longer count an original without a copy.*
- **CHANGED:** Separate mobile cache is off on new installs, and presets no longer turn it on. Existing sites keep their setting.*
- **NEW:** Separate mobile cache – a warning says when it is needed: only with different HTML for phones.*
- **NEW:** Unused CSS – a warning that deferred stylesheets without Generate Critical CSS cause a high CLS; the AI recommendation turns both on, and a new rule reports the risky setup.*

## 1.2.5
- **NEW:** LiteSpeed mode – the first page of the shop, categories and other listings has its own tag: a save refreshes it after the "Shared archives delay", the pages behind it are purged an hour later, also at Cloudflare.*
- **IMPROVED:** LiteSpeed mode – a save purges only the feeds of its post type, author and date archives only for blog posts.*
- **FIX:** LiteSpeed mode – the WooCommerce shop page carries the tag of the product archive, its purge reaches it.*
- **CHANGED:** LiteSpeed mode – a page is purged by its own tag only (OpenLiteSpeed ignores url= purges).*
- **FIX:** LiteSpeed mode – the tag of a page leaves out ignored query parameters (utm_, fbclid...): a page first opened from an ad link is purged by its address.*
- **IMPROVED:** LiteSpeed preload – a page the server answers with its old copy ("hit,stale") is asked again until the new one is ready, before the next page and the Cloudflare purge.*
- **FIX:** LiteSpeed preload – saved orders and other post types without pages no longer queue archive purges.*
- **NEW:** Filters zizi_cache_ls_deep_pages_delay, zizi_cache_ls_author_date_post_types and zizi_cache_ls_stale_purge (off).*

## 1.2.4
- **FIX:** LiteSpeed mode – .htaccess no longer has "CacheEnable public /": LiteSpeed stored answers WordPress never gave, such as the empty 403 of a firewall (AIOS) for an ad link with "|", and served them to every visitor. Only pages ZiziCache approved are cached; ad parameters (utm_, fbclid, gclid) still share one cache key.*
- **NEW:** LiteSpeed preload – one page at a time under a CPU budget (Page Cache, 25 % of one core by default, warning above 25 %) instead of parallel requests; it backs off when the server answers 429, 5xx or 508.*
- **NEW:** LiteSpeed preload – background refresh: a saved post or product is purged at once and fetched again seconds later, the homepage, shop and categories it appears on once per "Shared archives delay" (10 minutes) for all saves; visitors keep the cached copy until the new one is ready.*
- **NEW:** LiteSpeed preload – cached pages are fetched again before they expire; Cloudflare is purged in batches after the server has the new page.*
- **FIX:** LiteSpeed mode – a saved product no longer purges every product page (the tag of a post type is only on its archive).*
- **FIX:** WooCommerce (all cache modes) – a product saved without a change (an ERP or REST sync) purges nothing, nor does a product that is not published; a variation saved over the REST API purges the page of its product.*
- **IMPROVED:** WooCommerce (all cache modes) – a change of the stock quantity alone (an order, a stock sync) purges only the product page; the shop and categories follow a change of price, stock status, name, visibility or categories, as in LiteSpeed Cache.*
- **FIX:** LiteSpeed preload – a run left over without pages in its queue no longer blocks a new start (the AI connector answered 409).*
- **FIX:** LiteSpeed mode – no cache check pass that fetched every page of the site after each preload run and whenever the Overview was open.*
- **FIX:** Background jobs – a saved post no longer starts a wp-cron.php request of its own.*
- **CHANGED:** LiteSpeed preload – the settings Parallel threads, Delay between batches and Server load limit are gone (CPU budget).*

## 1.2.3
- **FIX:** LiteSpeed mode – the cache lifespan no longer purges the whole site (LiteSpeed and the Cloudflare zone) and crawls every page again; each page expires by its TTL and its next visit builds it. HTML on the Cloudflare edge can be up to two lifespans old, content changes still purge their pages.*
- **FIX:** Preload after a save – the shop, categories and homepage a save shares with other posts are preloaded at most once in 10 minutes (filter zizi_cache_auto_preload_shared_window); they are still purged on every save. A shop synced by an ERP or a REST client rendered the same categories after every product.*
- **FIX:** LiteSpeed mode – the file preload is off completely: the preload after a save, its queue, worker and watchdog and the refresh of expired files no longer fetch pages; a page purged after a save is built by its next visit, the LiteSpeed preload warms after a purge of the whole site. Until now only the full file preload checked the mode.*
- **IMPROVED:** Preload – the requests of the file preload carry "ZiziCache-Preload" in the User-Agent, as the LiteSpeed preload; they looked like visitors in the access log.*
- **SECURITY:** Preload requests no longer carry the login of an API client (WooCommerce REST key, application password) that started them; the HTTP login of a protected site still goes with them.*
- **FIX:** LiteSpeed preload – the direct route to the own server is checked on the home page of the site, not the domain root, and waits up to 15 s for it (an uncached home page after a purge took longer than 5 s); a site in a subfolder went through the CDN.*

## 1.2.2
- **FIX:** LiteSpeed preload – a run goes on by itself in the background after every part, also without visitors and with the admin closed; until now it waited for WP-Cron.*
- **FIX:** LiteSpeed preload – one worker at a time; a restarted run takes over from the old one.*
- **FIX:** LiteSpeed preload – starts after a plugin or theme update and after a plugin is activated or deactivated (they empty the LiteSpeed cache).*
- **FIX:** LiteSpeed – updates and plugin (de)activations in WP-CLI or a system cron empty the LiteSpeed cache and start the preload too.*
- **IMPROVED:** Preload – current browser User-Agents (Chrome 157, Safari 26 on iPhone); the refresh of expired pages no longer sends a bot User-Agent.*
- **IMPROVED:** Action Scheduler 4.2.0 (safer reading of stored schedules, unique actions per arguments).*
- **CHANGED:** Requires WordPress 6.9 or newer (Action Scheduler 4.2).*

## 1.2.1
- **FIX:** LiteSpeed preload – pages stored during the preload count as cached in the cache coverage.*
- **FIX:** LiteSpeed preload – the progress shows newly cached, already cached and not cached pages instead of the hit rate.*
- **FIX:** Overview – preloaded pages are compared with all pages of the site.*
- **FIX:** Overview – in LiteSpeed mode the preload queue replaces the hit rate and the chart shows the pages generated by WordPress.*
- **FIX:** Admin – the page stays in place when the WordPress menu is taller than the window.*
- **FIX:** Admin – no double percent signs; icons aligned with their text in Metrics.*
- **FIX:** Admin – pages work with locales such as Portuguese (AO90).*

## 1.2.0 – 2026-11-01
- **NEW:** Admin – completely new administration with light and dark theme, one Save changes bar for all settings, a settings snapshot on every save (last 10, restorable in History), no layout jumps while a page loads and a visible loading state while data load.*
- **NEW:** Editor – ZiziCache panel in the block editor and a box in the classic editor: switch off the page cache, optimizations or single features (lazy load, LCP preload, font preload) for one page.*
- **NEW:** Overview – tiles for cache coverage, hit rate, TTFB and preloaded pages, a chart of cache hits and misses (24 hours, 7 and 30 days) and a TTFB over time chart in Metrics.*
- **NEW:** Page cache – cache hit statistics (one byte per cached visit), on by default, switch in Page Cache.*
- **NEW:** Images – bulk conversion runs as a background job with progress, pause, resume and cancel; deleting originals asks for confirmation. WP-CLI: `wp zizi-cache images`.*
- **NEW:** Images – an upload no longer waits for AVIF/WebP; the copies are made in the background and the pages that use the image are purged afterwards.*
- **NEW:** Images – optional Delete product images that no longer exist (WooCommerce, off by default) and a tool that finds orphaned product images and deletes them after a preview.*
- **NEW:** Database – overview from a stored snapshot, cleanup in the background with progress, index recommendations with a dry run (shows the SQL and a time estimate), background apply and revert. WP-CLI: `wp zizi-cache db`.*
- **NEW:** Metrics – Core Web Vitals of real visitors in one new storage (periods 1, 7 and 28 days, FCP card); Redis page with a connection test and server statistics.*
- **NEW:** Settings – option to keep ZiziCache data on uninstall (default: delete, as before).*
- **NEW:** Quicklink – library updated to 3.0.2; speculation rules carry the tag `zizi-cache`; clearer descriptions of eagerness; warnings for prerender with eager, prerender without the analytics guard, Performance Lab, Cloudflare Speed Brain and the Instant Back/Forward plugin.*
- **NEW:** BFCache – the page explains how Chrome handles pages with no-store and detects the Instant Back/Forward plugin.*
- **NEW:** Developers – action `zizi_cache_config_saved($diff, $stored, $source)` with the changed settings.*
- **IMPROVED:** Translations – the whole administration in all 14 languages.*
- **IMPROVED:** Speed – clearing the cache, saving settings, database cleanup and external purges (Cloudflare, Varnish, hosting) run in the background; the request returns in well under a second.*
- **IMPROVED:** Loading – ZiziCache loads only the code the request needs (page, admin, REST, AJAX, cron); fewer files, no licence queries on visitor, admin, AJAX and heartbeat requests.*
- **IMPROVED:** Frontend – building an optimized page on a cache miss takes about half the time with fewer database queries; smaller plugin scripts and no Chart.js or Font Awesome in the package.*
- **IMPROVED:** Updates – one upgrade path with a lock for update and activation; no reset of the whole server OPcache, the object cache is flushed only for ZiziCache keys; deactivation removes all scheduled events, uninstall removes only its own wp-config lines.*
- **IMPROVED:** Purge – editing, renaming or deleting a term, removing a term from a post and editing a comment now purge the right archive or post; ZiziBlocks cache files are cleared too.*
- **IMPROVED:** Fonts – font statistics in a single option, only pages of your own site are accepted; the Google check, CDN check and YouTube/Vimeo thumbnails run in cron, not while a page is built.*
- **IMPROVED:** Logging – fewer log lines by default and no database writes for log throttling.*
- **CHANGED:** PHP – ZiziCache 1.2.0 requires PHP 8.2 (was 8.1). On an older PHP the plugin stays off, shows a notice and turns the page cache off; after going back to 1.1.7, save its settings once.*
- **CHANGED:** Downgrade – only to 1.1.7 is supported; deactivate ZiziCache before the downgrade. Per-page flags LCP and font preload have no effect there, and after restoring a 1.1.6 backup clear the cache and save the settings once by hand.*
- **CHANGED:** Defaults – new defaults for Google Fonts injection from statistics, animation auto-compose and auto preconnect apply to new installs only; existing sites keep their values.*
- **CHANGED:** Integrations – ZiziCache no longer changes the settings of Perfmatters, BuddyBoss or EWWW; it shows a notice with a Set it for me button instead. Earlier changes stay.*
- **CHANGED:** Images – thumbnail size options now work: disabled sizes apply on every upload (Media Library, REST, WP-CLI), and a switched-off option is saved.*
- **CHANGED:** Images – GIF images are no longer converted (the copy of an animated GIF showed its first frame only); GIF copies of earlier versions are removed after the update, except where the GIF itself was deleted.*
- **CHANGED:** Bypass cookies – a pattern is matched as part of the cookie name, not as a regular expression.*
- **CHANGED:** Log – the log file moved to `wp-content/zizi-cache-logs/`; after a downgrade the folder stays there.*
- **CHANGED:** Hook – `zizi_cache_update_config:after` now runs for every way of saving (fonts, images, Simple mode, database) and only when something changed; it is not called during the upgrade migration.*
- **CHANGED:** REST – the namespace `zizi-cache` (without `/v1`) is deprecated and answers with a Deprecation header (filter `zizi_cache_legacy_rest_namespace`); `/blocks/status`, `/blocks/{id}` and `/bfcache/diagnostics` too. Removal planned in 1.3.0.*
- **CHANGED:** REST – database cleanup, Optimize Database and applying index recommendations return a background job; database status no longer includes DB_USER and DB_HOST; old LCP, LoAF and TTFB beacons are ignored.*
- **CHANGED:** Quicklink – saved limits outside the allowed range are trimmed (throttle 1-10, limit 1-50, delay 0-5000, timeout 500-10000); prerender eagerness is fixed to moderate; Lazy Render method javascript works as hybrid.*
- **REMOVED:** Features – legacy JavaScript delay, legacy unused-CSS removal, the Lazy Render block panel in Gutenberg (it never worked), the image metadata validator, signature files (.zcsig), the Performance cards and Quick Actions (now in the Actions menu).*
- **REMOVED:** Settings – unused settings of earlier versions are removed from the stored configuration, together with leftover options and transients of removed features.*
- **REMOVED:** REST – 49 routes without a caller, among them `/cdn/status`, `/cdn/clear-cache`, `/performance-settings`, `/thumbnail/stats`, `/database-growth-history`, `/db-event-log`, `/ai/mcp/connections`, debug and test-cache of `/account` and `/license`, `media-indexer/scan` and `/stats`.*
- **REMOVED:** AJAX – `zizi_cache_test_redis`, `zizi_cache_save_html_settings`, `zizi_cache_save_html_exclusions`, `zizi_cache_get_html_exclusions`, `zizi_index_uploads`, `zizi_scan_ftp_images` and the purge/status actions of WPML, Weglot, Polylang, WooCommerce and WCML.*
- **REMOVED:** Hooks – filters `zizi_cache_exclude_from_delay:js`, `zizi_cache_preload_is_cacheable`, `zizi_cache_cache_key`, `zizi_cache_cache_include_queries`, `zizi_cache_should_load_admin_scripts` and actions `zizi_cache_purge_all`, `zizi_cache_font_data_updated` (they never ran or had no effect); some unused PHP methods are removed, a few stay as deprecated stubs until 1.3.0.*
- **FIX:** Page cache – the .htaccess rewrite base no longer picks up the language folder of TranslatePress, Polylang or WPML, which caused error 500 for visitors.*
- **FIX:** Cloudflare – the failover check works over HTTP and in WP-CLI.*
- **FIX:** Database – Optimize tables works on MySQL 8; the OPcache domain usage AJAX answers with JSON.*
- **FIX:** Settings – two saves at the same time no longer overwrite each other; a multi-line custom guard code can be saved; Quicklink prerender modes really prerender.*
- **FIX:** Page cache – pages with /wp-json/ or a file extension only in the query string and pages with sitemap in the slug are cached again; pages of logged-in users are not cached while BFCache is on.*
- **FIX:** Licence – the 24 hour and 7 day limits use UTC; the licence no longer expires on sites without working WP-Cron; SiteGround Speed Optimizer is detected.*
- **FIX:** CSS – relative URLs in CSS files are resolved correctly (../ above the web root, URLs with only a query).*
- **FIX:** Varnish – no PHP 8.5 deprecation notice when the purge answer is read.*
- **FIX:** Images – with Keep original files off, an original or thumbnail is deleted only when its WebP copy exists; before, browsers without AVIF could get AVIF or the full-size image.*
- **FIX:** Images – deleting originals on a Windows server no longer leaves the image with a broken file path.*
- **FIX:** Images – on WordPress 6.5 and 6.6, deleting originals now switches the image to its copy (before, it kept pointing at the deleted files); images left that way by 1.1.x and stored in the uploads folder are switched by the next bulk optimization.*
- **FIX:** Images – deleting originals works on a moved or copied site: the copies are checked on the disk, not by their stored paths.*
- **FIX:** Images – a format the server cannot write (AVIF without a working encoder, now checked by a test write) is no longer tried on every bulk run; with AVIF only, the bulk optimization says so.*
- **FIX:** Images – the statistics stay right after uploads, deleted images, deleted originals and deleted thumbnail sizes; before, they drifted until Recount statistics.*
- **FIX:** Images – an image replaced under the same file name (for example with Enable Media Replace) gets new copies; before, visitors kept getting the old image.*
- **FIX:** Images – deleting an image also deletes the backup of its original made before the original cleanup.*
- **FIX:** Uninstall – the image conversion data stored with the media are removed too.*
- **FIX:** Images – Hosting profile Automatic detects shared and managed hosting; before, it always used the whole time budget.*
- **SECURITY:** Plugin files – the src folder is blocked in .htaccess; the cache trash folder is protected by deny rules.*
- **SECURITY:** Images – deleting originals touches only files inside the uploads folder.*
- **SECURITY:** Fonts – font data from pages of other sites is ignored and anonymous writes are limited.*
- **SECURITY:** Quicklink – ignore patterns are data, no eval(); the custom guard code escapes the closing script tag; YouTube and Vimeo placeholder ids are checked and escaped.*
- **SECURITY:** Integrations – the AJAX purge actions without a nonce are removed.*
- **SECURITY:** ZiziBlocks – the rate limit of the public batch request trusts X-Forwarded-For only from a proxy.*

## 1.1.7
- **FIX:** Images – uploaded AVIF (and WebP) files keep their file extension; damaged attachments are repaired after the update.*
- **SECURITY:** Fonts – visitors can no longer purge the page cache through the font data AJAX action.*
- **FIX:** Polylang – no error while the page cache is written.*
- **FIX:** Pretty Links – no fatal error on init.*
- **FIX:** Images – palette GIFs convert without Imagick.*
- **FIX:** Database – the scheduled cleanup is scheduled again.*
- **FIX:** Overview – "Optimize Database" only optimizes tables, it no longer deletes revisions, trash and spam.*
- **FIX:** License – a "not valid" answer is no longer cached as valid for 24 hours.*
- **SECURITY:** Page cache – cached pages can no longer be downloaded directly from the cache folder (Apache, LiteSpeed; Nginx: updated snippet in NGINX_SECURITY_README.txt).*
- **FIX:** Update – the update steps also run after a downgrade from a newer version.*
- **FIX:** Uninstall – the cache trash folder of newer versions is removed too.*
- **FIX:** CDN – Enter in a CDN field no longer switches the CDN off.*

## 1.1.6
- **NEW:** WooCommerce – cached pages also for shoppers with a cart; cart fragments load only for them (Page Cache > Cache for Shoppers with a Cart).*
- **NEW:** Page cache – the notice names the plugin behind a foreign advanced-cache.php or WP_CACHE false and fixes both with one button.*
- **FIX:** Page cache – an empty advanced-cache.php left by WP Rocket no longer blocks the drop-in.*
- **FIX:** Page cache – WooCommerce guests with an empty session (side carts, filters) store the shared page.*
- **FIX:** Preload – after an update or a full purge the whole site is preloaded again, not only the homepage.*
- **FIX:** Preload – the next pages of paginated archives (/page/2/ ...) are preloaded right after the first one, also after a purge.*
- **FIX:** WooCommerce – the purge after a stock change of an order runs once, after the answer to the shopper.*
- **FIX:** ZiziBlocks – stock text kept, placeholders only in shared pages, nonces match the shopper session, no skeleton over variations.*
- **FIX:** JavaScript – data of wp_localize_script() is never delayed (variation swatches, button layout).*
- **FIX:** Redundancy – with jQuery Migrate removed, .load(fn), .size() and other calls removed in jQuery 3 keep working.*
- **FIX:** Admin – toggle panels keep their own status text.*

## 1.1.1
- **FIX:** Lazy load – a hero image served as a resized WebP/AVIF copy is kept out of lazy loading again.*
- **FIX:** Lazy load – the first-screen image no longer gets fetchpriority "low" when the browser picked its WebP/AVIF copy.*
- **FIX:** AI & MCP – the page check warns about a lazy image only when visitors saw it on the first screen.*
- **FIX:** AI & MCP – no recommendation to exclude an image that is the LCP of only a few views of its page.*

## 1.1.0 – 2026-10-20
- **NEW:** AI & MCP – connect Claude, ChatGPT, Cursor and other AI apps over MCP; off until an admin turns it on.*
- **NEW:** AI & MCP – sign-in with OAuth or an Application Password; WordPress Abilities on WordPress 6.9+.*
- **NEW:** AI & MCP – the AI reads cache status, settings and Core Web Vitals and runs the health checks.*
- **NEW:** AI & MCP – the AI checks pages of the site and recommends settings with their effect and risk.*
- **NEW:** AI & MCP – settings change only after a preview and your approval, with an automatic snapshot.*
- **NEW:** AI & MCP – purge and preload by page, URL or the whole site, with a dry run.*
- **NEW:** AI & MCP – bundled web performance skills, audit prompts and a skill ZIP for Claude Code, Claude.ai and Codex.*
- **NEW:** AI & MCP – lab runs of the AI are stored and compared before and after a change.*
- **NEW:** AI & MCP – presets and the self-test as AI tools; WP-CLI: wp zizi-cache self-test and preset.*
- **NEW:** Simple mode – one screen with three levels (Safe, Recommended, Aggressive) and 13 plain switches.*
- **NEW:** Simple mode – every change shows a preview first and can be undone for 24 hours.*
- **NEW:** Self-test – new installs check right away that the cache works and how much faster pages are.*
- **NEW:** Advanced mode – badges on settings to test after changing; advanced settings can be collapsed.*
- **NEW:** Page settings – a box in the editor turns off the cache or single optimizations for one page.*
- **NEW:** Optimizations – pause them for testing on the simple screen; ?zizi-cache-off shows a page without them.*
- **NEW:** JavaScript – quick exclusions for plugins whose scripts break when delayed.*
- **NEW:** Settings – history of changes, named snapshots and rollback with a preview.*
- **NEW:** Metrics – real-visitor LCP, INP, CLS, FCP and TTFB with 75th percentiles, trend and filters.*
- **NEW:** Metrics – slowest pages, LCP phases and element, CLS source, INP target and scripts by owner.*
- **NEW:** CSS – optional Used CSS for theme and page builder stylesheets (Elementor).*
- **NEW:** Fonts – optional fallback font metrics for self-hosted fonts, against layout shifts.*
- **NEW:** Fonts – font checks warn about heavy fonts and fonts without unicode-range.*
- **NEW:** JavaScript – optional delay of analytics and heatmap scripts until the first interaction.*
- **IMPROVED:** Metrics – measurement scripts load only on sampled page views.*
- **IMPROVED:** LCP – Auto LCP learns on every visit for 2 hours after caching, until the page has a record.*
- **IMPROVED:** Images – only the learned LCP image is preloaded with high priority; a text LCP preloads no image.*
- **IMPROVED:** Speculative Loading – rules come from the cache without WordPress; Quicklink only as a fallback.*
- **IMPROVED:** Preload – batch size follows the response time of the site (switch in Page Cache).*
- **IMPROVED:** Animation – only transform and opacity animations get their own layer.*
- **CHANGED:** Metrics – TTFB is part of the real-visitor metrics; its switch gives way to "Ignore query strings".*
- **CHANGED:** Settings – new installs start with the emoji script off, YouTube placeholders and local Gravatars on.*
- **CHANGED:** Page Cache – the debug header shows MISS [X] and HIT [404] instead of 404-CANDIDATE.*
- **CHANGED:** Database – the growth chart uses Chart.js; the Overview no longer loads a chart library.*
- **CHANGED:** Interface – updated sites stay in the Advanced mode; new installs start in the Simple mode.*
- **FIX:** CSS – stylesheets of active cookie consent plugins are left out of Used CSS, so the banner stays visible.*
- **FIX:** CSS – under a Content-Security-Policy that blocks inline handlers, stylesheets are not deferred (they would never load).*
- **FIX:** JavaScript – under a Content-Security-Policy that blocks inline scripts, scripts are only deferred, never delayed or inlined (a loader would never run).*
- **FIX:** CSP – inline styles are left untouched under a style hash policy, Gutenberg lazy blocks also honour a CSP in a meta tag.*
- **IMPROVED:** CSS – cookie banner styles of Pressidium, Cookie Notice and GDPR Cookie Compliance load without blocking rendering.*
- **FIX:** Preload – the WP-Cron preload queue no longer stops on a PHP error.*
- **FIX:** .htaccess – JavaScript sent as text/javascript is compressed and cached by browsers.*
- **FIX:** LiteSpeed – a second purge of the same URL in one WP-CLI or cron run reaches LSCache.*
- **FIX:** LCP – learned images survive the cache renewal; addresses with accents are matched.*
- **FIX:** Metrics – no console messages for visitors; Firefox can keep pages in the back/forward cache.*
- **FIX:** CSS – Remove Original keeps stylesheets that Used CSS could not read completely.*
- **FIX:** JavaScript – Defer all JavaScript also works on sites whose address contains a plugin or service name (zizicache.com).*
- **FIX:** Uninstall – removes the new tables and the Core Web Vitals cron events.*
- **SECURITY:** AI & MCP – the AI never reads or changes licence keys, passwords and tokens.*
- **SECURITY:** AI & MCP – sensitive settings (CDN URL, Redis host, alert e-mails...) cannot be changed by the AI.*
- **SECURITY:** AI & MCP – OAuth 2.1 with PKCE, hashed tokens and rate limits; a reused code revokes the sign-in.*
- **SECURITY:** AI & MCP – Origin check, read-only sign-ins cannot change settings, answers are never cached.*
- **SECURITY:** AI & MCP – page checks fetch only this site; visitor data reaches the AI as untrusted text.*
- **SECURITY:** Metrics – measurement beacons accept only allowed fields, small payloads and limits per IP.*

## 1.0.19
- **FIX:** TranslatePress – preload and the purge after saving use the real language addresses.*
- **FIX:** Dashboard – cached pages and Pages Not Cached cover every language version of multilingual sites.*

## 1.0.18
- **FIX:** Preload – no longer deletes WP-Cron events that other plugins schedule while it runs.*
- **FIX:** Page Cache – first-time visitors get cached pages again on shops with CURCY (Multi Currency for WooCommerce).*

## 1.0.17
- **FIX:** LiteSpeed – WooCommerce notices, recently viewed products and emptied carts no longer end up in cached pages.*
- **FIX:** LiteSpeed – pages of a full cart are cached only while the cart cookies match the cart.*

## 1.0.16
- **SECURITY:** REST API – settings responses never contain the Cloudflare token or passwords.
- **SECURITY:** Page Cache – ?cache_bust rebuilds a cached page only for logged-in users.
- **SECURITY:** ZiziBlocks – prices by role or tax location are no longer shared between visitors.
- **SECURITY:** Images – deleting an image also deletes its WebP/AVIF copies, which stayed public.
- **SECURITY:** Fonts – font downloads always verify certificates, also behind a local proxy.
- **SECURITY:** Statistics – rate limits of the public CWV, LCP and font beacons can no longer be bypassed.
- **SECURITY:** Admin – Redis and Memcached password fields are never prefilled by the browser or printed into the page.
- **SECURITY:** License – license keys are masked in the plugin log and the browser console.
- **NEW:** Page Cache – notice when WordPress does not load the page cache.
- **NEW:** Settings – login for loopback requests on Basic Auth protected sites.
- **NEW:** LiteSpeed – notice when OpenLiteSpeed needs a restart for new rules.
- **NEW:** CSS – Purge Critical CSS button and a list of protected stylesheets.
- **NEW:** JavaScript – switches for the Animation CLS guard.
- **FIX:** Admin – one permission rule for the settings page, REST and AJAX.
- **FIX:** Page Cache – drop-ins and wp-config lines of other plugins stay untouched.
- **FIX:** Page Cache – switching it off and on handles the drop-in and WP_CACHE correctly.
- **FIX:** Page Cache – the drop-in is written in one step, no HTTP 500 during a save.
- **FIX:** Page Cache – Bypass URLs match the same pages everywhere and purge them on save.
- **FIX:** Page Cache – tablets and the Sec-CH-UA-Mobile hint get one consistent variant.
- **FIX:** Page Cache – gclid, fbclid and utm values no longer end up in cached pages.
- **FIX:** Page Cache – Include Query Strings win, generic parameters can be removed.
- **FIX:** Page Cache – WOOCS, CURCY and WCML currencies get their own cached pages.
- **FIX:** Page Cache – WooCommerce geolocation with page caching works.
- **FIX:** Page Cache – 304 and gzip follow the visitor's variant and Accept-Encoding.
- **FIX:** Page Cache – expired pages are refreshed in the background again.
- **FIX:** Purge – pagination, comment pages, host aliases and proxied hosts are cleared.
- **FIX:** Purge – menu, widget, theme, template and permalink changes clear the cache.
- **FIX:** Preload – after a save the page, homepage and archives are warmed again, also with LiteSpeed.
- **FIX:** Preload – saving in the editor no longer waits for the preload.
- **FIX:** Preload – URLs added during a running batch are warmed right after it.
- **FIX:** LiteSpeed – a pending purge is no longer lost when another purge follows.
- **FIX:** Settings – saves purge only when a setting changes cached pages, on every tab.
- **FIX:** Settings – consistent defaults, 0 and empty values keep their type.
- **FIX:** Admin – settings save with a renamed plugin folder, subfolder or plain permalinks.
- **FIX:** LiteSpeed – one cache mode everywhere, X-LSCACHE only with a constant.
- **FIX:** LiteSpeed – cache off, engine switch, WP-CLI and cron purge LSCache.
- **FIX:** LiteSpeed – bypass, password, commenter and logged-in cookies skip LSCache.
- **FIX:** LiteSpeed – Separate Mobile Cache keeps a mobile copy, the preload warms it.
- **FIX:** LiteSpeed – status codes, redirects and BFCache for logged-in users respected.
- **FIX:** LiteSpeed – purge everything clears this site only.
- **FIX:** LiteSpeed – pauses its mode while the LiteSpeed Cache plugin is active.
- **FIX:** LiteSpeed – the dashboard no longer crawls the site every 15 seconds.
- **FIX:** LiteSpeed – the preload respects its switch and the load per CPU core.
- **FIX:** .htaccess – WordPress in a subfolder, strict Apache servers, no needless rewrites.
- **FIX:** Hosting – Kinsta, WP Engine, SiteGround, Varnish, APO and Nginx Helper are purged.
- **FIX:** Cloudflare – disconnect works, the cache rule follows edge caching.
- **FIX:** Cloudflare – the edge cache follows the site's cache headers and keeps mobile, tablet and desktop apart.
- **FIX:** Cloudflare – excluded pages, searches, currency cookies and Bypass Cookies are no longer cached at the edge.
- **FIX:** Cloudflare – purged after the page cache files, old pages no longer return to the edge.
- **FIX:** CDN – relative URL rewrite no longer breaks foreign and JSON URLs.
- **FIX:** CSS – Remove Original removes only stylesheets covered by Critical CSS.
- **FIX:** CSS – Critical CSS per page type, template and device, rebuilt after changes.
- **FIX:** CSS – encoded inline backgrounds, media queries and CSP nonces survive.
- **FIX:** CSS – Early Hints preload only the Critical CSS the page uses.
- **FIX:** CSS – Remove Original keeps code highlighting styles (Prism, highlight.js).
- **FIX:** Fonts – icon fonts, variable weights and theme-hosted fonts are kept.
- **FIX:** Fonts – the Auto-Optimizer purges only when the fonts change.
- **FIX:** Fonts – fonts the site declares itself (Elementor Pro Custom Fonts) are not loaded again from Google.
- **FIX:** CSS/JS – third-party files are named after the whole URL.
- **FIX:** Images – originals are deleted only where the server can replace them.
- **FIX:** Images – delivery follows frontend serving, CDNs and refused checks.
- **FIX:** Images – hero images, lazy backgrounds, sizes and picture preloads.
- **FIX:** Images – the image width set by the author is kept, only the height follows the file.
- **FIX:** Images – WebP and AVIF also for images with Elementor thumbnails in srcset.
- **FIX:** IFrame – lazy loaded maps and videos load again.
- **FIX:** JavaScript – After page load no longer holds clicks on working buttons.
- **FIX:** JavaScript – Elementor libraries and add-ons are no longer delayed until after load.
- **FIX:** JavaScript – with deferred jQuery, ready handlers see the inline data (WPForms).
- **FIX:** JavaScript – jQuery Migrate runs after a jQuery loaded from a CDN or theme.
- **FIX:** Optimization – an error during optimization sends the page unoptimized instead of HTTP 500.
- **FIX:** Statistics – debug modes no longer measure every visitor.
- **FIX:** Remove Redundancy – public REST of known plugins stays open, list with exceptions.
- **FIX:** ZiziBlocks – blocks load on old cached pages and with a blocked REST API.
- **FIX:** Speculative Loading – header on all pages, Analytics Guard IDs save.
- **FIX:** Pages Not Cached – pagination, real preload status, sites on a port.
- **FIX:** Database – the scheduled cleanup keeps recent revisions and spam.
- **FIX:** Object Cache – Memcached without the PHP extension no longer breaks the site.
- **FIX:** SiteGround – its options are switched off once, only with the page cache on.
- **FIX:** Admin – panel status texts stay translated after switching an option.
- **FIX:** License – deleting the plugin releases this site's activation, copies of the site keep theirs.
- **FIX:** License – a failed activation shows the real reason and where the license is active.
- **FIX:** License – activation succeeds only when the license is saved, errors offer a retry.
- **FIX:** License – deactivation tells whether the activation was really released.
- **FIX:** License – an outage of the license server no longer switches off a recently checked license.
- **FIX:** Uninstall – works from WP-CLI, removes all plugin tables, no PHP warning.
- **CHANGED:** Page Cache – CDN-Cache-Control only with the Cloudflare integration.
- **CHANGED:** Settings – removed options that had no effect.
- **CHANGED:** Cloudflare – the edge cache TTL follows the page cache expiration.
- **CHANGED:** Images – new installs keep the original images.
- **CHANGED:** Remove Redundancy – jQuery Migrate stays on for new installs and in builders.
- **CHANGED:** Database – new installs have no scheduled cleanup.
- **IMPROVED:** Translations – all new texts in 15 languages.

## 1.0.15
- **SECURITY:** Page Cache – preload flags in a request no longer store POST or logged-in pages.
- **SECURITY:** Page Cache – unlocked password protected posts are no longer cached for other visitors.
- **SECURITY:** Page Cache – pages with a commenter's name, email or pending comment are no longer cached.
- **SECURITY:** Page Cache – pages rendered with a WooCommerce cart or session are no longer stored for everyone.
- **SECURITY:** Page Cache – currency and cart variants are no longer served as a language fallback.
- **SECURITY:** Page Cache – cached pages for logged-in roles need a signed role cookie.
- **SECURITY:** 404 Cache – the shared 404 page comes only from an anonymous visit and is refreshed after the lifespan.
- **SECURITY:** Admin – settings only for allowed roles, no Cloudflare token or passwords in the page or the drop-in.
- **SECURITY:** Files – the log and the preload database get secret names and are blocked in .htaccess.
- **FIX:** Page Cache – a saved LiteSpeed mode on a server without LiteSpeed no longer switches the page cache off.
- **FIX:** Page Cache – DONOTCACHEPAGE and no-store/private headers are respected, also on LiteSpeed.
- **FIX:** Page Cache – cache expiration 0 no longer switches the cache off while old pages stay online.
- **FIX:** LiteSpeed – hit rate and preload behind Cloudflare check this server, not the Cloudflare copy.
- **FIX:** ZiziBlocks – visitors with items in the cart get a fresh page, the cart count shows their cart.
- **FIX:** ZiziBlocks – nonce fields keep their action, forms no longer fail after hydration.
- **FIX:** Prefetch – Speculation Rules and Quicklink skip links with parameters (add to cart, log out).

## 1.0.14
- **SECURITY:** REST API – the CWV page summary is admin-only.
- **NEW:** JavaScript – "Defer all JavaScript" with two strategies: Defer (default) and After page load.
- **NEW:** JavaScript – inline scripts can be deferred in their original order (off when CSP forbids it).
- **NEW:** Image Converter – delivery check with status on the Images screen and an Nginx snippet.
- **FIX:** JavaScript – inline code keeps its order and dependencies (jQuery, Mergado).
- **FIX:** JavaScript – ZiziCache and Quicklink scripts no longer block the parser.
- **FIX:** JavaScript – muted autoplay videos are never held back.
- **FIX:** Update – saved settings without a default were reset on every update.
- **FIX:** Settings – the fallback save keeps all JavaScript options.
- **FIX:** LCP – images below the fold are no longer preloaded after scrolling.
- **FIX:** LCP – without a separate mobile cache, each device preloads only its own LCP image.
- **FIX:** Image Converter – every visitor gets AVIF, WebP or the original, independent of the page cache.
- **FIX:** Image Converter – without originals, attachments point to WebP.
- **FIX:** .htaccess – only ZiziCache sections are changed, other rules stay untouched.
- **FIX:** Uninstall – no longer removes other plugins' rules or causes HTTP 500.
- **FIX:** Deactivation – .htaccess rules are removed in "auto" cache mode too.
- **FIX:** REST API – the logged-in-only restriction no longer blocks ZiziBlocks, CWV or the Store API.
- **FIX:** REST API – endpoints work with plain permalinks.
- **FIX:** ZiziBlocks – the current minified client is loaded without WP_DEBUG.
- **FIX:** Translations – Spanish and Slovak corrected, new texts translated into all languages.
- **FIX:** ZiziBlocks user blocks and relative times use English source texts (other languages showed Czech).
- **CHANGED:** JavaScript "Load on user interaction" and CSS "On Interaction" are retired; settings are migrated.
- **CHANGED:** JavaScript – the third-party panel delays only widgets (chat, embeds, video).
- **IMPROVED:** JavaScript – analytics, consent managers and pixels are never delayed.
- **IMPROVED:** Image Converter – "Vary: Accept" only where needed, so Cloudflare caches images again.
- **IMPROVED:** Lazy loading supports `<picture>`.
- **IMPROVED:** Frontend scripts back off when the REST API is blocked.

## 1.0.13

- **FIX:** License – plan name and details refresh on every validation.
- **FIX:** LiteSpeed – automatic URL purges now reach LSCache.
- **FIX:** Update – the drop-in is regenerated when its template changes.
- **FIX:** Automatic purge – pages are purged after all save handlers, no stale content.
- **FIX:** Elementor – saving templates or Site Settings purges all pages.
- **FIX:** CSS – exclusions work with CSS minification on.
- **FIX:** Critical CSS – self-hosted Google Fonts CSS is no longer inlined.
- **FIX:** Priority hints – an existing fetchpriority="high" is kept.
- **FIX:** WooCommerce – cart fragment and asset options work with late theme enqueues.
- **FIX:** ZiziBlocks – variable products can be added to the cart again.
- **FIX:** ZiziBlocks – FOX currency prices redraw on cached pages.
- **FIX:** Preload – one worker at a time, browser Accept header for WebP/AVIF.
- **FIX:** Preload – the self-ping parameter no longer clashes with other plugins.
- **IMPROVED:** LiteSpeed – the purge loopback no longer renders the homepage.
- **IMPROVED:** CSS – theme and Elementor layout styles are never deferred; the list is filterable.
- **IMPROVED:** ZiziBlocks – new "Dynamic Prices" switch.
- **IMPROVED:** Preload runs without WP-Cron.
- **IMPROVED:** Preload and REST work on stagings behind HTTP Basic Auth.

## 1.0.12

- **SECURITY:** Hardening of the CDN screen, the LCP beacon and the page-cache drop-in.
- **FIX:** PHP 8.1 – fatal error in the LCP handler.
- **FIX:** Page cache – browsers no longer keep cached HTML for a year.
- **FIX:** Image Converter – conversion runs for REST and WordPress 7.1 client-side uploads.
- **FIX:** Image Converter – warning for relative image URLs.
- **FIX:** Thumbnails – disabled sizes are skipped in WordPress 7.1 uploads.
- **FIX:** Database – scheduled cleanup respects the settings and trash retention.
- **FIX:** Database – the monthly schedule works.
- **FIX:** Database – transient cleanup with a persistent object cache.
- **FIX:** Database – Highcharts loaded twice.
- **FIX:** Dashboard – cached and total page counts match; excluded pages shown.
- **FIX:** Cloudflare – purge retries and failover DNS paging.
- **FIX:** Cloudflare – connection test uses the GraphQL Analytics API.
- **FIX:** Preload – starts immediately from the dashboard, skips non-viewable post types.
- **FIX:** Fonts – no preload of deleted self-hosted font files.
- **IMPROVED:** Image Converter – originals are kept until all sub-sizes exist.
- **IMPROVED:** Database – safer index recommendations, with revert.
- **IMPROVED:** Database – autoload summary lists the largest options.
- **IMPROVED:** Cloudflare – coalesced full purges, 100-URL batches, current token permissions.
- **IMPROVED:** License – failed validations are cached, not retried on every request.
- **IMPROVED:** Logging – less log noise.
- **Compatibility:** tested with WordPress 7.1.

## 1.0.11
- **SECURITY:** Privilege escalation — the plugin's REST API management endpoints (configuration, database cleanup, license, Cloudflare/CDN credentials, cache purge) no longer grant access to the `editor` role. Access is restricted to administrators, still filterable via `zizi_cache_allowed_roles`. Previously an Editor holding a standard `wp_rest` nonce could reach administrator-level controls.*
- **SECURITY:** Page cache path hardening — the `advanced-cache.php` drop-in now rejects path-traversal sequences and null bytes in the request path and `Host` header, sanitizes the role/currency cookies used in cache filenames (matching the language-cookie handling), and enforces a realpath containment check before serving a cached file. Prevents unauthenticated cross-directory cache disclosure in file-based cache mode.*
- **SECURITY:** Log exposure — the bundled `.htaccess` (and the generated Nginx snippet) now also block rotated log backups (`zizi-log.log.1`, `.log.2`, …), not just `*.log`.*
- **SECURITY:** Updater now requires the update package to be delivered over HTTPS before download (defense-in-depth against downgrade/MITM); non-standard hosts are logged.*
- **FIX:** License activation on hosts that block URLs containing the word "license" — the license REST endpoints are now also served under a neutral `/account/*` path (used by the admin UI), with the historic `/license/*` path kept as a backward-compatible alias. Some hosting Nginx security rules deny any request URL containing `license`, which previously blocked activation/deactivation entirely on those hosts.*
- **IMPROVED:** Removed leftover debug logging from the optimization hot path (`OptiCore`) that ran on every uncached front-end request.*
- **IMPROVED:** `zizi_cache_db_size_history` is no longer autoloaded on every request.*

## 1.0.10
- **FIX:** Add missing width/height — wrong author-declared dimensions no longer distort images. The optimiser now reads the actual file dimensions for every `<img>`, compares them to the author's `width`/`height` attributes, and when the ratio drifts beyond 5% it rewrites the attributes to match reality before the defensive `aspect-ratio` style is emitted. Previously a raw-HTML widget such as `<img width="380" height="380" src="logo-500x205.webp">` would have its 1:1 aspect ratio locked in by the injected style and the image would render permanently distorted. Author-declared values within tolerance, images without an HTML width/height, SVG, and Gravatar still flow through their existing paths unchanged.*
- **FIX:** Add missing width/height — when the actual file dimensions cannot be determined (broken URL, remote image without `img_width_height_remote_fetch`, unsupported format), the optimiser no longer emits an unverified `aspect-ratio` style; it leaves the `<img>` exactly as the author wrote it instead of risking a wrong locked ratio.*
- **NEW:** Add missing width/height — per-image opt-out via `class="zizi-skip-dims"` plus two filters: `zizi_cache_dimension_drift_tolerance` (default 0.05, range 0–1) and `zizi_cache_skip_dimension_correction($skip, $src, $html_w, $html_h, $file_w, $file_h)`.*
- **IMPROVED:** `Image::get_dimensions()` is now memoised per request, so repeated images (logos, icons, featured-image grids) hit `getimagesize()` only once per page render — keeps the new verify-on-every-image pass essentially free.*

## 1.0.9
- **FIX:** Cloudflare Emergency Failover — The toggle on the CDN tab is now persisted on save.*
- **FIX:** Early Hints (103) — CSS and image hints are no longer fed from dead config keys. CSS hints now auto-pick the CSSEnhanced critical/RUCSS bundle for the current URL type when CSS optimization is active, and image hints auto-pick the detected LCP image from the runtime LCP transient — but only when the plugin's master `img_preload` toggle is ON, so disabling image preload in the plugin disables image Early Hints. Font hints additionally honour the manual UI list (`fonts_preload_manual_urls`) instead of only the cron-populated legacy key.*
- **FIX:** Cloudflare Edge Cache Purge Parity — When the user (or any setting save / Font pipeline rebuild / WP-CLI flush) triggered `Purge::purge_pages()` while Cloudflare integration was active, the Cloudflare Manager only purged the homepage on the edge — the rest of the site stayed stale until manual intervention. The hook handler now mirrors LiteSpeed/APO behaviour: a `purge_pages` call with no targeted list maps to a Cloudflare `purge_everything`, so users see the same behaviour they're used to from LS mode and File-based mode.*
- **NEW:** Cloudflare Edge Invalidation on Site-Structure Events — The Cloudflare Manager now hooks `wp_update_nav_menu`, `update_option_sidebars_widgets`, `switch_theme`, `customize_save_after`, `activated_plugin`, `deactivated_plugin`, and `update_option_permalink_structure` (parity with `LiteSpeedSupport`) and issues a full edge purge. Debounced within one request so a burst of structure changes collapses to a single API call. Only fires when `cdn_type=cloudflare` AND `cloudflare_enabled=true` — LiteSpeed-only and File-based installs are not affected.*
- **IMPROVED:** Early Hints visibility — Reminder for site owners: HTTP 103 only surfaces in DevTools when Cloudflare's "Early Hints" feature (Speed → Optimization → Early Hints) is enabled on the zone. The plugin emits `Link: <…>; rel=preload` headers which Cloudflare then promotes to 103 responses.*

## 1.0.8
- **FIX:** WPML Compatibility — Resolved a fatal `Too few arguments to function ZiziCache\Plugins\Integrations\WPML::handleTranslationUpdate()` (`TypeError`, expected 3, got 1) that blocked content editing on any site running WPML alongside ZiziCache.*

## 1.0.7
- **IMPROVED:** PSI Mobile — Self-Host Then Minify Order — `OptiCore::process_output` now runs `JavaScript::self_host_third_party_js()` BEFORE `JavaScript::minify()`. Previously the order was reversed, so any third-party JS (Google Analytics, Microsoft Clarity, Cloudflare beacon, Hotjar, …) freshly downloaded by self-host bypassed the minify pass entirely on first request and was served verbatim. With this swap, every cross-origin script that the user opted to self-host now gets compressed in the same pipeline pass — typically a 25-35% size reduction on non-minified analytics blobs. Recovers a portion of the "Minify JavaScript — Est savings of 10 KiB" Lighthouse audit.*
- **NEW:** PSI Mobile — Inline Script Minify Pass — Added `JavaScript::minify_inline_scripts()` to compress every `<script>…body…</script>` block ≥200 chars (Elementor settings, gtag config, dataLayer, _wpUtilSettings, plugin localized data). Skips application/ld+json, text/template, scripts tagged data-zizi-ignore / data-no-optimize / data-cfasync, and base64-encoded delay-inline payloads. Closes the gap that left WordPress inline blocks unminified despite the master JS minify toggle being ON. Wired into the pipeline immediately after `JavaScript::minify()`.*
- **NEW:** PSI Mobile — Inline wp-hooks + wp-i18n Into `<head>` — Added `JavaScript::inline_wp_core_scripts()` which inlines the bodies of `<script id="wp-hooks-js">` and `<script id="wp-i18n-js">` (typically 3-4 KiB each) directly into the document head. These two scripts are emitted on virtually every WordPress 5.5+ page, are flagged "must stay sync" because Elementor / WooCommerce / Yoast / Rank Math all rely on their globals before any inline `*-before` companion block, and on slow mobile each costs a full HTTP RTT — about 800ms of pure latency for ~7 KiB of payload that fits in a single TCP packet. Inlining eliminates both round-trips and the parser-blocking time is unchanged (the bytes still need to be parsed either way). Per-file cap default raised to 20 KiB with a hard floor of 16 KiB so the typical WP-core webpack bundles (~10 KiB each) always fit; users with stricter needs can lower via filter. Options: `inline_wp_core_scripts` (default ON), `inline_wp_core_max_bytes` (default 20480, enforced floor 16384).*
- **NEW:** PSI Mobile — Auto-Preconnect for External Origins — `OptiCore::auto_preconnect()` scans the final HTML for cross-origin URLs in `<script src>`, `<link href>`, `data-src`, `data-zizi-delay-src` attributes, tallies distinct hosts, and emits `<link rel="preconnect" crossorigin>` for the top 4 busiest origins right after `<meta charset>`. Skips origins already preconnected, the page's own origin, and user-listed excludes. Warms DNS + TCP + TLS handshakes for third-party assets (Clarity multi-subdomain, Cloudflare beacon, Gravatar, embedded media) ~250 ms before the first request to them. Options: `auto_preconnect` (default ON), `auto_preconnect_max` (default 4, range 1-8), `auto_preconnect_min_count`, `auto_preconnect_excludes`. Filter `zizi_cache_auto_preconnect_origins` lets advanced users reshape the discovered list.*
- **IMPROVED:** PSI Mobile — Quicklink Init Deferred Past LCP/CLS Window — `quicklink-integration-fixed.js` no longer calls `quicklink.listen()` / `quicklink.prerender()` immediately at script execution. The decision logic (Speculation Rules support detection, network conditions, user config parsing) still runs inline as before — only the actual IntersectionObserver setup + initial viewport scan is wrapped in `addEventListener('load', () => requestIdleCallback(init, {timeout: 4000}))`. Removes Quicklink's contribution to TBT and "Long main-thread tasks" Lighthouse audit. The Speculation Rules → Quicklink fallback chain and all admin settings (quicklink_enabled, quicklink_mode, prerender flags, ignores, origins, data-saver respect, …) are honored verbatim. Disable via `quicklink_defer_init = false` for legacy behavior.*
- **IMPROVED:** CLS Guard — Animation Post-Paint MutationObserver Fallback — `Animation::auto_compose()` now appends a tiny (~680-byte) post-paint observer to `<body>` that watches the DOM for elements receiving an `animationName` at runtime — catching the four common cases the static `@keyframes` scanner cannot reach: JS-injected keyframes (GSAP / AOS / dynamically-added `<style>` blocks), `style="animation-name:…"` set after paint, animations whose stylesheet is hosted on a CDN that bypassed local file resolution, and animations triggered by class changes via MutationObserver `attributeFilter: ['style','class']`. Idempotent (`data-zc-ap` marker prevents reprocessing); runs once on `load` then observes incrementally. Option `cls_animation_post_paint_observer` (default ON).*
- **IMPROVED:** Font `font-display` Strategy — Idempotent Per-Face Override — `Font::inject_display_swap()` rewritten to operate per `@font-face` block instead of stripping every declaration globally and re-injecting. The Font Display Strategy setting remains a deliberate global force-set knob: every face ends up with the user-configured value (default `swap`). Idempotent skips and surgical rewrites: faces whose `font-display:` already matches the configured value are left untouched (no churn on already-correct CSS); faces with a different declaration get that one declaration replaced in-place; faces without any declaration get the configured value injected at the start of the block. The Critical CSS dedup pipeline (`filter_used_css_by_critical_fonts`) was aligned to read the configured value from SysConfig instead of hardcoding `swap`. Net effect: same final CSS as before, but the rewrite preserves the surrounding block structure and is safe to re-run repeatedly on already-processed CSS.*
- **NEW:** CLS Guard — Animation Auto-Compose — Eliminates Lighthouse "Non-composited animations" warnings and the layout-shift contribution they cause. ZiziCache now scans every `@keyframes` block in inline `<style>` tags and linked stylesheets, identifies frames that mutate layout-affecting properties (`transform: scale|translate|skew|rotate|matrix`, or `width`/`height`/`top`/`left`/`right`/`bottom`/`margin`/`padding` with units), then finds every selector that drives those keyframes via `animation:` or `animation-name:` and emits a tiny inline `<style id="zc-anim-promote">` before `</head>` that promotes them to a GPU compositor layer (`will-change: transform, opacity; backface-visibility: hidden`). Detection is property-based (NOT name-based) so it works with ANY user-named keyframe — `pulse`, `aurora-main`, `hero-bounce-v2`, minified `kg-zw3`, animate.css, Elementor effects, custom CSS — across thousands of WordPress sites without per-site whitelisting. Capped at 200 selectors per page to prevent CSS bloat. New options: `cls_animation_compose` (master toggle, default ON), `cls_animation_use_contain` (also emit `contain: layout paint`, default OFF — opt-in because it can affect fixed/absolute descendants), `cls_animation_compose_excludes` (selectors to skip). Filter `zizi_cache_animation_compose_excludes` available. Tested on zizicache.com: cleared the 8-element non-composited animations audit and removed the desktop CLS contribution from the `pulse` and `aurora-main` keyframes.*
- **FIX:** Image Preload — Eliminated false-positive `<link rel="preload" as="image">` on pages where the LCP element is text/heading or the only image lies below the fold. New STRICT mode (default ON, option `img_atf_strict_mode`) skips the legacy N-count "first 3 images" bootstrap when no real ATF viewport data exists for the (URL, device) combination. First visit pays a one-frame lazy-load cost; subsequent visits get accurate preloads from the JS scanner. Filter `zizi_cache_atf_strict_mode` available to override.*


## 1.0.6
- **FIX:** Language-Aware Cache — Now respects the user’s Language Priority configuration by mapping detected language subsets to their corresponding position within the priority chain.
- **FIX:** LiteSpeed Mode — Resolved an issue where LiteSpeedPreload::process_results() never triggered increment_warmed_count(), causing the warmed pages counter to remain at 0 even after hundreds of successful warmups. Verification tests confirmed the fix.
- **IMPROVED:** LiteSpeed Hit Rate Visualization — Reworked the layout from a 4-column grid with numeric counters above labels (HIT / MISS / No-C / UNK) into a unified vertical stack layout matching the rest of the Cache Status card design.

## 1.0.5

- **FIX:** Cache Status in Dashboard Overview — Now correctly reflects warmed pages and recalculates totals without artificial inflation. UI adjustments applied for both LS mode and File-Based mode.
- **NEW:** Translations — Added support for the following locales: cs_CZ — Čeština (Česká republika), sk_SK — Slovenčina (Slovensko), de_DE — Deutsch (Deutschland), es_ES — Español (España), fr_FR — Français (France), it_IT — Italiano (Italia), pt_BR — Português (Brasil), nl_NL — Nederlands (Nederland), pl_PL — Polski (Polska), sv_SE — Svenska (Sverige), da_DK — Dansk (Danmark), nb_NO — Norsk Bokmål (Norge), ja — 日本語 (日本), tr_TR — Türkçe (Türkiye).
- **IMPROVED:** Optimization > CSS Loading Strategy — Added new method “Remove Original”.
- **IMPROVED:** Optimization > CSS Loading Strategy — Enhanced “Exclude Stylesheets” and “Never-Remove Selectors” handling.
- **IMPROVED:** Image Optimization — Improved detection of images for preload in Above The Fold.
- **IMPROVED:** Optimization > Delay All JavaScript — Added new method “Load when idle”.

## 1.0.4
- **FIX:** Image Optimization — sizes="auto" is now only emitted on lazy-loaded images, where it is valid per the HTML spec. Eager/ATF images no longer include a redundant or invalid sizes attribute; their responsive srcset remains and defaults to 100vw.
- **FIX:** Image Optimization — loading="eager" is no longer written to output HTML; eager loading is the browser default and the attribute was unnecessary.
- **FIX:** Image Optimization — fetchpriority and decoding attributes are now consistently placed at the end of the <img> tag, after src, srcset, and sizes.
- **FIX:** Image Optimization — In N-count bootstrap mode (first visit without ATF/viewport data), only 1 <link rel="preload" as="image"> is generated for the hero/LCP image instead of all N eager images.
- **IMPROVED:** Image Optimization — On fresh pages without ATF data, the first N images (default N=3) remain eager as a bootstrap; only the first image receives a preload with fetchpriority="high" until real viewport data is collected.

## 1.0.3
- **IMPROVED:** Preload watchdog now recovers from stale or overdue cron events when DISABLE_WP_CRON=true is used with infrequent server-side cron schedules.
- **FIX:** Permanent MISS on multilingual sites (WPML, Polylang, TranslatePress) resolved; cache drop-in now correctly reads language-specific cache files (e.g. index-cs.html.gz).
- **SECURITY:** Language cookie values are sanitized before use in filesystem paths to prevent path traversal.
- **NEW:** NagataPress multilingual integration — translated URL variants (/en/, /de/, etc.) are added to preload and auto-purge queues.
- **NEW:** Cookie-based multilingual preload warming for WPML, Polylang, and TranslatePress via zizi_cache_preload_lang_cookies.
- **NEW:** Dashboard "Cache files" badge displaying total .html.gz page cache files (including language variants).
- **IMPROVED:** Cache Status page now correctly detects cached pages on multilingual setups with language-suffixed filenames.
- **NEW:** Speculation Rules API served via Speculation-Rules HTTP header pointing to /wp-json/zizi-cache/v1/speculation-rules (Cloudflare Speed Brain compatible).
- **NEW:** REST endpoint /wp-json/zizi-cache/v1/speculation-rules with correct MIME type and cache headers.
- **NEW:** Cache drop-in (advanced-cache.php) now sends Speculation-Rules header on cache HITs.
- **FIX:** Analytics prerender guard updated to use document.prerendering instead of deprecated API.
- **FIX:** Prerender guard added to LCP, TTFB, and LoAF detectors to prevent execution during prerender phase.
- **FIX:** Quicklink analytics guard script is no longer emitted empty when no analytics IDs are configured.
- **REMOVED:** Invalid Speculation Rules admin toggles ("Require HTTPS", "Anonymous IP") removed.
- **FIX:** Font auto-detect now includes CSS fallback scanning for @font-face woff2 URLs.
- **FIX:** Font Intelligence REST endpoint now persists data for anonymous visitors.
- **FIX:** Admin "Auto-detected Font URLs" field now falls back to fonts_preload_urls when needed.
- **FIX:** Font statistics reset now fully clears all related options and transients.

## 1.0.2
- **IMPROVED:**  Enhanced license validation.
- **IMPROVED:**  UI improvements in the plugin dashboard.

## 1.0.1
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
