# Superior Ice Adventures — Deploy & Setup

Live subdomain: **https://superioriceadventures.webstarbusinessservices.com**

cPanel-ready PHP site (HTML/CSS/PHP + MySQL). No Node/Composer required at runtime.

## Local (XAMPP)

1. Ensure Apache + MySQL are running in XAMPP.
2. Confirm `includes/db.config.php` exists (gitignored). For first setup:
   ```
   copy includes\db.config.example.php includes\db.config.php
   ```
   Default local credentials: `root` / empty password / database `superior_ice_adventures`.
3. Create tables and seed admin + categories:
   ```
   C:\xampp\php\php.exe database\setup.php
   ```
4. Open: `http://localhost/superior-ice-adventures/`
5. Admin: `http://localhost/superior-ice-adventures/admin/`
   - Username: `admin`
   - Password: `admin123` (change after first login)

Local URLs stay under `/superior-ice-adventures/`. Canonical tags, sitemap, and mail From use the live subdomain.

## Production — superioriceadventures.webstarbusinessservices.com

### Git deploy (recommended)

1. In cPanel → **Git Version Control**, create/clone this repo on the server.
2. Confirm [`.cpanel.yml`](.cpanel.yml) is in the repo root (required filename with leading dot).
3. Deploy target is **`$HOME/superioriceadventures.webstarbusinessservices.com/`** — not the main `public_html` site. Change `DEPLOYPATH` if Domains → Document Root shows a different folder.
4. Create `includes/db.config.php` **once** on the server (never overwrite from Git — deploy excludes it).
5. Pull / Deploy from cPanel when you push to GitHub. Deploy syncs site files and skips `node_modules/`, `_raw/`, and local DB credentials.

### Manual / first-time checklist

1. In cPanel → **Domains** / **Subdomains**:
   - Subdomain: `superioriceadventures`
   - Domain: `webstarbusinessservices.com`
   - Document root: `superioriceadventures.webstarbusinessservices.com` (default)
2. Enable **SSL** (AutoSSL / Let’s Encrypt) for `superioriceadventures.webstarbusinessservices.com`
3. Upload project files (exclude local `includes/db.config.php`; skip `_raw/` and `node_modules/`) — or use Git deploy above
4. In cPanel → **MySQL Databases**:
   - Create a database (e.g. `yourprefix_sia`)
   - Create a user and grant **All Privileges**
5. In phpMyAdmin:
   - Select that database
   - Import `database/schema-cpanel.sql`
   - Import `database/seed-cpanel.sql`
6. On the server only, create `includes/db.config.php` from `db.config.example.php` using the **full prefixed** cPanel DB name and user
7. Confirm `mod_rewrite` is on. `.htaccess` forces HTTPS on this subdomain

After SSL propagates, the site should load at:

- https://superioriceadventures.webstarbusinessservices.com/
- https://superioriceadventures.webstarbusinessservices.com/admin/
- https://superioriceadventures.webstarbusinessservices.com/sitemap.php
- https://superioriceadventures.webstarbusinessservices.com/robots.txt

## Content

All marketing copy lives in `includes/config.php`. Identity, services, About, FAQ, gallery, and contact intros are filled for launch.

Site URL settings:

- `$site_domain` = `superioriceadventures.webstarbusinessservices.com`
- `$site_url` = `https://superioriceadventures.webstarbusinessservices.com`
- `$mail_from` = `noreply@webstarbusinessservices.com`

### Image pipeline (Soldotna-style)

1. Drop originals in `_raw/images/<folder>/` (see `_raw/README.md`).
2. Run:
   ```
   npm install
   npm run media:images
   ```
3. Optimized `.webp` files land in `assets/img/...` (max width 2400px, quality 82).

Do not upload `_raw/` or `node_modules/` to cPanel — only the processed `assets/img/` WebPs.

## Contact form

Set `$contact_form_to` (and `$email`) in `includes/config.php`. Submissions use PHP `mail()` from `noreply@webstarbusinessservices.com`. On shared hosting, confirm the host allows outbound mail; create that mailbox or an alias in cPanel if the host requires it. Some hosts require SMTP — switch later if needed.

## Admin blog

- Single admin role (draft / publish)
- Featured article toggle
- Categories CRUD
- Markdown body (headings, lists, bold/italic, links)

## Fonts & colors

- Fonts: Inter + Fraunces (Google Fonts)
- Palette: off-white `#fbf9f5`, flannel red `#710F10`, black `#0a0a0a`
