# WP Synthetic Data Tool

A single-page, browser-based tool for populating a WordPress site with realistic synthetic content — posts, pages, users, comments, WooCommerce products, media, and custom post types — for QA, load, and template testing.

Everything runs client-side in the browser and talks to the target site over the WordPress REST API. Nothing is installed on the site and no data is sent to any third-party server (other than optional placeholder images from picsum.photos).

## Usage

1. Open `index.html` in a browser (no build step, no server required).
2. **Connection details** — enter the site URL and a WordPress [Application Password](https://make.wordpress.org/core/2020/11/05/application-passwords-integration-guide/) (Users → Profile → Application Passwords in WP Admin). Credentials are never persisted — they only live in memory in the open browser tab.
3. Click **Check connection** to verify the REST API is reachable and the credentials are valid.
4. **Content theme** — pick a content theme (e.g. California Surf Shop, B2B SaaS Product Blog, Farm-to-Table Restaurant). This determines the categories, tags, and body copy used for generated content. The "Site label" field is for your own reference only and doesn't affect generation.
5. **Content volume** — choose a preset (Small / Medium / Large) or set exact counts for posts, pages, users, comments, and products.
6. **Media & extra content types** (optional):
   - Enable placeholder image generation to fetch images from picsum.photos and attach them to posts, pages, and products as featured/product images.
   - Add custom post types by REST base slug (e.g. `event`, `testimonial`) to seed generic title/body entries. The post type must be registered with `show_in_rest` and reachable at `wp/v2/<rest_base>`.
7. Click **Review & confirm** to see the content plan, then **Approve & populate site** to run it.
8. Progress is shown step-by-step as content is created; a summary and a link to the site are shown on completion.

## Content presets

| Preset | Posts | Pages | Users | Comments | Products |
|--------|------:|------:|------:|---------:|---------:|
| Small  | 10    | 5     | 5     | 25       | 10       |
| Medium | 75    | 15    | 25    | 200      | 50       |
| Large  | 300   | 40    | 100   | 800      | 150      |

Presets just fill in the count fields — any value can be edited directly.

## Notes

- Content is created sequentially over the REST API, so large volumes (500+ items) can take several minutes and may hit host rate limits.
- WooCommerce products are only created if WooCommerce is installed and active on the target site; all other content is created regardless.
- Requesting placeholder images adds one extra request per item and noticeably slows down the run.
- Authentication uses HTTP Basic Auth with the WordPress Application Password, sent directly from the browser to the site's REST API.

## Requirements

- Target site must be a WordPress install with the REST API enabled and reachable from the browser (no CORS blocking cross-origin requests, if running this tool from a different origin than the target site).
- An Application Password for a user with sufficient capabilities to create posts, pages, users, comments, and (optionally) products.
- WooCommerce, if generating products.
