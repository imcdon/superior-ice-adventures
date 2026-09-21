# Raw media

Drop original photos here. This folder's images are gitignored; `_raw/README.md` stays tracked.

## Expected structure

```
_raw/
  images/
    hero/         # slide-1, slide-2, slide-3 (home carousel)
    services/     # inland-splake-fishing, lake-superior-burbot,
                  # lake-superior-lake-trout, ice-shack-rentals
    about/        # story-1, story-2, story-3, guide, team-1, team-2, team-3
    gallery/      # 01 … 06 (or more)
    categories/   # ice-fishing-tips, trip-reports, gear
    textures/     # flannel-red-black (header texture source, optional)
```

File stems should match the paths in `includes/config.php` (lowercase `.webp` after processing).

## Processing

Raw images may use mixed extensions (JPEG, PNG, WebP, HEIC/HEIF, etc.). The script normalizes collisions (e.g. `slide-1.JPG` + `slide-1.heic`), picks one source file, and emits a single **lowercase `.webp`** per logical name under `assets/img/`.

```bash
# Process all images (resize to 2400px wide, WebP quality 82)
npm install
npm run media:images
```

`npm run media` is an alias for `media:images`.

Outputs go to `assets/img/...` matching the paths referenced from `includes/config.php`, seed data, and page templates.

Keep `assets/img/favicon.svg` unless you replace it deliberately. Header flannel currently uses `assets/img/textures/flannel-red-black.png`; re-run media after dropping a source in `_raw/images/textures/` if you want a WebP version (update CSS to match).
