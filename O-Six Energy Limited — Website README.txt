# O-Six Energy Limited — Website README

**Purpose:** a clear, friendly, step-by-step guide for anyone (including non-technical editors) to understand, edit, update, and deploy the site.
This README explains file structure, how to change text/images/colors, make the contact form live, add projects, basic SEO, a troubleshooting checklist, and best practices.

---

## Quick start (one-page summary)

1. Put all HTML files in the **same folder**.
2. Open `index.html` in a browser to preview the site.
3. To edit content, open the HTML file you want (e.g., `about.html`) in a text editor (Notepad, VS Code, Sublime, or even Microsoft Word with plain-text mode).
4. Replace image placeholders (in `images/` or inline URLs) with your own photos, keeping recommended sizes.
5. When ready to go live, upload the whole folder to your web host or GitHub Pages.

---

## Files included (what each file does)

* `index.html` — Home page (hero, about blurb, services preview, projects preview)
* `about.html` — Company background, mission, vision, values, leadership
* `services.html` — Detailed list of services
* `projects.html` — Portfolio grid linking to project detail pages
* `project-single.html` — Template for individual project detail (dynamic and/or simple variants were provided). The dynamic version reads `?id=1` parameters.
* `contact.html` — Contact page with form and embedded map
* `robots.txt` — Basic crawler instructions
* `sitemap.xml` — Sitemap for search engines (update domain when live)
* `images/` (recommended) — folder for your images (hero, projects, favicon). NOTE: some pages used external placeholder images — it's best to replace with local files in `images/` for stability.

> **Important:** Many pages are self-contained with all CSS and JS embedded inside the `.html` files (inline styles/scripts). That means each page works by itself and there is no single `style.css` used in some versions — check your copy to know which variant you have. The README below assumes the inline version you asked for.

---

## Editing content (non-technical step by step)

### Tools you can use

* Simple: Notepad (Windows), TextEdit (Mac — set to plain text).
* Friendly: Visual Studio Code (free), Sublime Text, Atom.
* For quick visual edits: any WYSIWYG HTML editor (e.g., BlueGriffon) — but be careful not to remove code.

### How to edit text on a page

1. Open the `.html` file in a text editor.
2. Find the text you want to change (use the editor’s find feature — Ctrl/Cmd+F). Example: to change hero headline search for `<h2>Building the Foundation for Renewable Energy</h2>`.
3. Replace the text and save the file.
4. Open the file in your browser (double-click `index.html`) to preview.

### How to change the logo/site name in header

* In each HTML file locate the `<header>` block near the top. Replace the text inside `<h1>O-Six Energy Ltd</h1>` or the `.logo` element.

### How to edit navigation links

* In the `<nav>` area at top of each file, update `href="..."` attributes to point to the proper pages. Keep file names consistent (e.g., `about.html`).

---

## Changing images (hero, project photos)

### Where images live

* If a page uses `assets/images/...` or `images/...`, store your new images in that folder and use the same filename or update the `<img src="...">` path inside the HTML.

### Recommended image sizes & formats

* **Hero banner**: 1920 × 900 px (or 1600 × 700). WebP if possible; JPG/optimized PNG otherwise. Aim < 300 KB.
* **Project thumbnails**: 600 × 400 px (or 400 × 300). Aim < 150 KB.
* **Project single/banner**: 1000 × 400 px.
* **OG image (social share)**: 1200 × 630 px.
* **Favicon**: 64 × 64 px (PNG). Also add 32×32/16×16 as needed.

### How to set an image

* Replace the `src` attribute:

```html
<!-- before -->
<img src="https://via.placeholder.com/1000x400?text=Mini+Grid" alt="Mini Grid">

<!-- after (local image) -->
<img src="images/mini-grid-plateau-2023.jpg" alt="Mini Grid - Plateau" width="1000" height="400">
```

* Include `width` and `height` attributes when possible to avoid layout shifting.

### Compressing images

* Use tools: [Squoosh.app](https://squoosh.app), TinyPNG, ImageOptim. Prefer WebP for best compression.

---

## Updating colors, fonts, and brand

Because styles are inline (inside `<style>` at top of each file), search the file for `:root` or color hex codes:

* Primary color: `--primary` or `#0b3d91` / `#004d26` depending on version.
* Accent color: `--accent` or `#f1c40f` / `#f9a825`.
* To change site color globally (inline variants): update the color variables in every file (or use find/replace across files). Example:

```css
:root {
  --primary: #1b5e20; /* change to your brand primary color */
  --accent: #f9a825;
}
```

Fonts: Google Fonts were referenced in some pages, e.g.:

```html
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
```

Keep or replace with your preferred Google Font. If you remove external fonts, browsers will fall back to system fonts.

---

## Making the contact form functional (non-technical options)

The HTML contact form posts to `#` by default (no backend). You can choose one of these quick options:

### Option A — use a form service (no coding)

**Formspree** or **Formsubmit.co** allow form POST to an endpoint and forward to email.

**Formspree example**

1. Sign up at [https://formspree.io/](https://formspree.io/) and create a form endpoint (you’ll be given a URL).
2. In `contact.html` change the `<form>` tag:

```html
<form action="https://formspree.io/f/yourFormID" method="POST">
  <!-- fields -->
</form>
```

3. Add any hidden inputs Formspree requires and test. They handle email delivery.

**Pros:** No server, easy for non-tech users.
**Cons:** Free tier limits; need to verify email.

### Option B — simple PHP mail (requires PHP on host)

If your host supports PHP, rename `contact.html` to `contact.php` and use a PHP mail handler.

Example (very basic — must be secured in production):

```php
<?php
if($_SERVER['REQUEST_METHOD'] == 'POST'){
  $name = htmlspecialchars($_POST['name']);
  $email = htmlspecialchars($_POST['email']);
  $message = htmlspecialchars($_POST['message']);
  $to = 'info@osixenergy.com';
  $subject = "Website enquiry from $name";
  $body = "Name: $name\nEmail: $email\n\nMessage:\n$message";
  mail($to, $subject, $body, "From: $email");
  header('Location: thankyou.html');
  exit;
}
?>
<!-- then include the usual HTML form with method="post" -->
```

**Warning:** PHP mail requires proper server configuration and validation. If you use PHP, add server-side validation and spam protection (reCAPTCHA).

### Option C — Email link (no form)

Replace the form with a mailto link:

```html
<a href="mailto:info@osixenergy.com?subject=Website%20Enquiry">Email us</a>
```

Simple, but user experience varies (opens user email client).

---

## Adding or editing projects

### If your `projects.html` links to separate `project-single.html` files:

* Create `project1.html`, `project2.html`, etc. Copy the `project-single.html` template and update:

  * Title (`<h2>` / hero text)
  * Featured image path
  * Description text
  * Metadata (client, location, year)

Update `projects.html` links:

```html
<a href="project1.html"> ... </a>
```

### If you use the **dynamic single page** (one `project-single.html?id=1` approach):

* Open `project-single.html` (the dynamic version with JS `projects` object).
* Add a new entry inside the `projects` object:

```js
projects[4] = {
  title: "New Project Title",
  hero: "images/project4-hero.jpg",
  image: "images/project4.jpg",
  meta: "<div><strong>Client:</strong> ...</div>",
  description: "<p>...</p>",
  stats: "<div class='stat-item'>...</div>"
}
```

* In `projects.html`, update the link to pass `?id=4`:

```html
<a href="project-single.html?id=4">...</a>
```

---

## SEO & metadata (easy for non-techs)

Edit the top of each HTML file in the `<head>`:

* `<title>` — keep concise: `O-Six Energy Limited | Civil Works & Solar Infrastructure`
* `<meta name="description" content="...">` — 120–160 characters summary.
* OpenGraph for social sharing:

```html
<meta property="og:title" content="O-Six Energy Limited">
<meta property="og:description" content="Civil construction for durable mini-grid & off-grid solar infrastructures.">
<meta property="og:image" content="images/og-image.jpg">
```

* Sitemap and robots are already included; update `sitemap.xml` URL to match your domain.

---

## Accessibility & content best practices

* All images should have `alt="..."` describing the image (helpful for screen readers).
* Use semantic headings: `<h1>` once per page, then `<h2>`, `<h3>`.
* Ensure color contrast (text vs background) is readable — the color scheme provided has contrast, but verify when you change colors.
* Add `lang="en"` in the `<html>` tag (already set).
* Add `aria-label` attributes only when needed (e.g., forms).

---

## Responsive behavior & mobile navigation

* The pages already include media queries that collapse the menu on small screens.
* If you change the header layout, make sure the mobile menu toggle (`☰`) JavaScript function remains present (search for `toggleMenu()` or similar in the HTML file and keep that script).

---

## Deployment options (simple, non-technical)

1. **Upload via FTP / cPanel**

   * Use your hosting control panel or FTP client (FileZilla). Drop all HTML files (and `images/` folder) to the public folder (often `public_html/` or `www/`).
   * Visit your domain to verify.

2. **GitHub Pages (free for static sites)**

   * Create a GitHub repo, push the HTML files to the `main` branch, enable GitHub Pages under repo Settings (choose root for static site).
   * Your site will be served at `https://username.github.io/repo/`. Update `sitemap.xml` and robots accordingly.

3. **Netlify / Vercel (drag & drop or connect repo)**

   * Drag the site folder to Netlify deploy page or connect repo. They provide a live URL and free SSL.

---

## Backups & versioning (very important)

* Keep a copy of the entire folder before making big changes. Create a `backup-YYYYMMDD.zip`.
* For ongoing updates, consider using Git/GitHub (even if you are not a developer — GitHub Desktop app has a friendly UI).

---

## Troubleshooting (common issues and fixes)

### 1. Page shows plain text / no styling

* Problem: missing CSS or styles overridden.
* Fix: If inline variant — ensure `<style>` is present inside `<head>`. If separate stylesheet variant — check `<link rel="stylesheet" href="assets/css/style.css">` path, and ensure the CSS file exists in that exact path.

### 2. Images not showing

* Problem: incorrect path or filename mismatch.
* Fix: Verify the image file is in the `images/` folder and `src` path matches the filename exactly (case-sensitive on many servers). Try opening the image directly in browser (e.g., `sitefolder/images/mini.jpg`).

### 3. Contact form not sending

* Problem: form action is `#` or no backend.
* Fix: Use Formspree (no server) or add PHP handler on server. See "Making the contact form functional" above.

### 4. Links give 404 when live

* Problem: file name typo or missing file.
* Fix: ensure the exact file name exists (e.g., `about.html` not `About.html` on Linux hosts).

### 5. Map not displaying

* Problem: Google Maps embed may require API key or usage limits.
* Fix: Replace iframe with your site’s proper embed or remove if you don’t want maps.

---

## Adding new pages (example)

1. Copy an existing page (e.g., `about.html`) and rename to `team.html`.
2. Edit the content (title, headings) and the header nav links (add link to `team.html`).
3. Save and test by opening the new file.

---

## Checks before going live (quick checklist)

* [ ] All pages have correct titles and meta descriptions.
* [ ] Replace all placeholder images with optimized project photos.
* [ ] `sitemap.xml` has the correct full URLs for your domain.
* [ ] `robots.txt` references the correct sitemap URL.
* [ ] Contact form connected and verified (test sending).
* [ ] Site previewed on phone, tablet, desktop (check layout & nav).
* [ ] Spellcheck and grammar pass for all copy.
* [ ] Accessibility: images have alt text, contrast OK.

---

## Example edits (copy-paste snippets)

**Change site email in footer (search for email):**

```html
<!-- replace in each HTML file -->
<p>&copy; 2025 O-Six Energy Limited. All Rights Reserved. | info@osixenergy.com</p>
```

**Add a new project link (projects.html):**

```html
<a href="project6.html" class="project-card">
  <img src="images/project6.jpg" alt="Project 6">
  <div class="project-info">
    <h4>New Project Title</h4>
    <p>Short description...</p>
  </div>
</a>
```

**Add a new entry to the dynamic single page (project-single.html):**

```js
projects[4] = {
  title: "New Project",
  hero: "images/project4-hero.jpg",
  image: "images/project4.jpg",
  meta: "<div><strong>Client:</strong> ...</div>",
  description: "<p>Long description here</p>",
  stats: "<div class='stat-item'>...</div>"
};
```

---

## Security & privacy notes

* Do **not** store sensitive data in plain HTML.
* If collecting user emails/messages, implement spam protection (Google reCAPTCHA) and a data handling/privacy policy.
* Keep the contact email monitored.

---

## Maintenance schedule (recommended)

* Weekly: Check form deliveries (if any).
* Monthly: Compress new images, run spellcheck, verify contact details.
* Quarterly: Update portfolio and news.
* Yearly: Update copyright year.

---

## Changelog (template)

Add a `CHANGELOG.md` in the folder and keep entries like:

```
### 2025-11-02
- Launched V1 of site with 5 pages: index, about, services, projects, contact.
- Added dynamic single project viewer.

### 2025-11-15
- Replaced project images and updated meta descriptions for SEO.
```

---

## Where to get help

* For basic edits you can contact your web admin or a local web developer.
* For managed hosting or email, contact your hosting provider support.

---

## Final tips for non-tech editors

* Always make a **backup** before major edits.
* Use descriptive file names for images: `mini-grid-plateau-2023.jpg` (helps SEO).
* Keep content short, clear, and use bullets for services and facts.
* When in doubt, copy a page and test changes locally (open HTML in a browser) before uploading.

---

If you’d like, I can:

* Produce a ready-to-print PDF version of this README.
* Create a `CHANGELOG.md` or `CONTRIBUTING.md`.
* Package the site into a ZIP you can download.

Which would you like next?
