Advanced WordPress caching and performance optimization plugin. Set It. Cache It. Done.

ZiziCache is a performance plugin for WordPress that combines file‑based page cache, optional object cache (Redis or Memcached), and an optimization pipeline that rewrites HTML, CSS, JavaScript, images, fonts, and iframes before output.

**Highlights:**

* **Multi‑layer caching** – Page Cache (File Based or native LiteSpeed Cache), object cache (Redis/Memcached), and OPcache tools
* **Optimization pipeline** – CSS/JS minify, Critical CSS + CSS Loadign strategy, JS defer/delay, HTML tweaks, font optimization + preload, video/iframe placeholders, lazy loading, priority hints
* **Image Converter** – AVIF/WebP conversion on upload or in bulk, frontend format serving, auto registred uploaded images via FTP, Thumbnails size management.
* **Core Web Vitals monitoring** – LCP/LoAF/TTFB collection, admin real time banner
* **Speculative loading + Quicklink** – prefetch/prerender rules with exclusions
* **BFCache safety** – session‑aware headers for safe back/forward cache
* **Database tools** – cleanup and Database Index Recommendations with allow‑listed apply
* **Font Optimization** - ZiziCache collects font usage statistics to optimize font loading. These statistics help determine which fonts and weights are actually used on your site.
* **Cloudflare CDN & Pull CDN** – edge rules, purge routines, URL rewriting

== Frequently Asked Questions ==

= What makes ZiziCache different from other caching plugins? =

ZiziCache blends multi-layer caching with automation engines that handle database tuning, Core Web Vitals telemetry, BFCache safety, and CDN control from a single interface. You get verified Database Index Recommendations, LoAF/TTFB monitoring, font and media intelligence, and speculative navigation so you don't need a patchwork of niche add-ons.

= How does the Database Index Recommendations feature work? =

The plugin inspects WordPress core tables via `SHOW INDEX`, detects missing indexes that match real query patterns, rates them (high/medium/low), and stores the results in a transient for one hour. The admin UI surfaces a summary, table size, estimated improvement, and the exact ALTER TABLE statement, while the `apply-index-recommendations` REST endpoint executes the allowlisted SQL. The January 2025 analysis validated this end-to-end workflow.

= Who can apply the recommended indexes? =

By default, the REST endpoints rely on `Authority::rest_api_permission_callback`, which grants access to administrators and editors (filterable) that pass the standard `wp_rest` nonce and capability checks. Use the provided filters if you want to restrict the feature to `manage_options` or custom roles before allowing ALTER TABLE operations.

= Does ZiziCache work with WooCommerce? =

Yes. Cart, checkout, account, and other dynamic endpoints are automatically excluded from page cache, BFCache rules respect WooCommerce scripts, and the preloader avoids sensitive URLs so storefronts stay accurate while still benefiting from performance boosts.

= Can I use ZiziCache with CDN services? =

Absolutely. ZiziCache supports pull CDNs plus a native Cloudflare integration that manages Edge Cache Rules, targeted purges, conflict detection with other cache plugins, and automatic fallbacks if the CDN is unavailable.

= Does ZiziCache support multilingual websites? =

Yes, the cache and optimization layers are compatible with WPML, TranslatePress, and Weglot, and include URL-based exclusions to keep language switchers and localized assets consistent.

= How does font optimization work? =

Font Intelligence tracks the fonts that actually render above the fold, generates Google Fonts CSS, assigns preload tags, enforces variant budgets, and runs the Font Auto-Optimizer on a WP-Cron schedule so you never serve unnecessary font payloads.

= What are Priority Hints and how do they help? =

Priority Hints let browsers know which resources to fetch first. ZiziCache automatically applies `fetchpriority` and `importance` attributes to critical images, fonts, and responsive preload targets so LCP elements arrive sooner even on congested networks.

= Does ZiziCache work on shared hosting? =

Yes. The plugin is tuned for shared hosting limits with lightweight cron jobs, transient caching for database analysis, optional Redis offloading, and safe fallbacks when advanced server modules are unavailable.
