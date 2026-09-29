# Deploying www.cjtee.com

## What you're uploading
Upload the contents of this folder — not the folder itself — to the root ("public_html" / "www" / site root) of your web host:

```
index.html
favicon.ico
favicon-16x16.png
favicon-32x32.png
apple-touch-icon.png
icon-192.png
icon-512.png
site.webmanifest
robots.txt
sitemap.xml
images/carla-headshot.jpg
```

All paths inside index.html are relative or root-relative (e.g. `/favicon.ico`, `images/carla-headshot.jpg`), so the file structure above must be preserved exactly — the `images` folder sits next to `index.html`, not inside another folder.

## A. Hosting — general steps (works for any standard host: Bluehost, SiteGround, Hostinger, Netlify, Vercel, GitHub Pages, etc.)

1. Sign up for hosting if you haven't already, or use hosting bundled with your domain registrar.
2. Get access to the site's file manager or FTP/SFTP credentials (host dashboard → "File Manager," "FTP Accounts," or similar).
3. Upload every file listed above into the site's document root (commonly `public_html/`, `www/`, or `htdocs/`).
4. Confirm `index.html` sits directly in that root folder — most hosts automatically serve `index.html` as the homepage of a directory, so no extra configuration is needed.
5. Visit your host's temporary URL (most give you one, like `yourhost.com/~yoursite` or a Netlify/Vercel subdomain) to confirm the site loads before pointing the domain at it.

### If you'd rather use a specific host
- **Netlify / Vercel / Cloudflare Pages** (free static hosting): drag-and-drop the whole folder in their dashboard, or connect a GitHub repo. No server config needed — these hosts autodetect `index.html`.
- **Traditional cPanel host** (Bluehost, HostGator, SiteGround, Hostinger, Namecheap hosting): use the File Manager or an FTP client (FileZilla) to upload into `public_html/`.
- **GitHub Pages**: push these files to a repo, enable Pages in repo settings, point Pages at the root of the `main` branch.

## B. Connecting your custom domain (www.cjtee.com)

Exact steps depend on whether your domain registrar and your host are the same company or different companies.

### If domain and hosting are the same provider
Most hosts (Bluehost, Hostinger, etc.) auto-configure this when you buy hosting and domain together. Just confirm in the host dashboard that `cjtee.com` and `www.cjtee.com` are both listed as "assigned" or "primary domain" for this hosting account.

### If domain (registrar) and hosting are different companies
You need to point the domain's DNS at the host. Two common methods — use whichever your host's setup instructions specify:

**Method 1 — Nameserver change (most common, simplest)**
1. In your host's dashboard, find the nameservers they want you to use (looks like `ns1.hostname.com`, `ns2.hostname.com`).
2. Log into your domain registrar (wherever you bought cjtee.com — GoDaddy, Namecheap, Google Domains, etc.).
3. Find "Nameservers" or "DNS Settings" for the domain.
4. Replace the existing nameservers with the ones your host gave you.
5. Wait for propagation (see timing note below).

**Method 2 — A record + CNAME (if you're keeping the registrar's DNS and just pointing records at the host)**
Add these DNS records at your registrar:

## E. DNS records to add

| Type | Host / Name | Value | Notes |
|---|---|---|---|
| A | `@` (root domain) | Your host's IP address (they'll give you this — e.g. `192.0.2.10`) | Points cjtee.com to your server |
| CNAME | `www` | `cjtee.com` (or the host's provided CNAME target, e.g. `yoursite.netlify.app`) | Points www.cjtee.com to the same place |

- If using **Netlify/Vercel/Cloudflare Pages**, they'll give you an exact CNAME target (like `yoursite.netlify.app`) instead of an IP — use that instead of the A record above, following their dashboard's domain-connect instructions.
- If your host wants the root domain as a CNAME too (not all DNS providers allow this), they may instead give you an "ALIAS" or "ANAME" record — same idea, follow their instructions.
- Propagation can take anywhere from a few minutes to 24–48 hours. Don't panic if it doesn't work instantly.

### Making sure both `cjtee.com` and `www.cjtee.com` work
Decide which one is canonical (this site's `<link rel="canonical">` and Open Graph tags are set to `https://www.cjtee.com/`), then set up a redirect from the other version to it. Most hosts have a one-click "redirect www to non-www" or vice versa option in the dashboard; if not, ask your host support to set up that forwarding rule.

### HTTPS / SSL
Nearly all modern hosts (including all the ones named above) offer free SSL certificates (often via Let's Encrypt), usually with a one-click "Enable HTTPS" or "Enable SSL" toggle in the dashboard. Turn this on — browsers flag non-HTTPS sites as "Not Secure," which undercuts a professional portfolio. If your host doesn't do this automatically, it's worth switching hosts.

## F. Remaining technical issues / things to know

1. **Contact "form" is actually a mailto link.** The "Email Carla" buttons open the visitor's own email client (`mailto:tee.cjp@gmail.com`). This works everywhere with no backend, but it depends on the visitor having a configured email client — some visitors on shared or work computers won't. If a proper on-page contact form is wanted later, that requires a small form-backend service (e.g., Formspree, Netlify Forms) — not included here because it wasn't part of the current design.
2. **Google Fonts is a live external dependency.** The site loads Manrope and DM Sans from `fonts.googleapis.com`/`fonts.gstatic.com` at run time. This requires visitors to have normal internet access to Google's font CDN (true for virtually everyone) — no action needed, but it's worth knowing this is the one piece not fully self-hosted. If ever wanted fully offline-capable, the font files could be downloaded and self-hosted instead.
3. **Favicon is a generated placeholder.** Since no logo exists yet, the favicon/app icons use plain "CT" initials in the site's teal. Swap in a real logo later by replacing `favicon.ico`, `favicon-16x16.png`, `favicon-32x32.png`, `apple-touch-icon.png`, `icon-192.png`, and `icon-512.png` with new versions at the same filenames and sizes.
4. **Update `sitemap.xml` / `robots.txt` / canonical tag if the domain changes.** All three currently point to `https://www.cjtee.com/`. If a different domain is used, update the `<loc>` in `sitemap.xml`, the `Sitemap:` line in `robots.txt`, and the `og:url` / `og:image` / `canonical` tags in `index.html`.
5. **No analytics installed.** Nothing tracks visits right now. Adding Google Analytics/Plausible/etc. later is a small, optional addition to the `<head>`, not required for the site to work.
6. **Single page, no routing.** This is one HTML file with in-page anchor links (`#help`, `#work`, etc.) — there's no multi-page routing to configure, and no server-side rendering or build step is needed. Any static host works.
