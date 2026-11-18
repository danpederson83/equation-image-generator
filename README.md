# Equation Maker

A free, single-page equation editor that turns equations into images for Google Slides (or any other app).

- Type equations the way you would in Word or MathType: `/` makes a fraction, `^` an exponent, `_` a subscript.
- Button palettes for structures, symbols, units, Greek letters, and common physical science formulas.
- **Copy image** puts a high-resolution, transparent PNG on the clipboard. Paste it straight into a slide.
- **Download PNG** / **Download SVG** save files. Google Slides doesn't accept SVG, so use PNG there. SVG works in PowerPoint, Keynote, Word, Canva, and on the web.
- Your recent equations are remembered in your browser so you can click one to edit it again.

Everything runs in your browser. Nothing is uploaded anywhere.

## Using it

Open `index.html` in Chrome, Edge, or Firefox. It needs an internet connection to load the editor and math fonts.

### Hosting it for free with GitHub Pages

1. On GitHub, go to the repository's **Settings → Pages**.
2. Under **Build and deployment**, set **Source** to *Deploy from a branch*, pick the branch and the `/ (root)` folder, then click **Save**.
3. After about a minute the site is live at `https://danpederson83.github.io/equation-image-generator/`. Bookmark it.

## Deploying to your own server (pedersonlab.com)

This is a **static site**: no build step, no server-side code, no database. Deploying means serving the files in this repo from a web server. Only `index.html` is needed at runtime; `README.md` is documentation.

### Requirements

- **HTTPS is required.** The "Copy image" button uses the browser Clipboard API, which only works on `https://` pages (or `localhost`). On plain `http://` copying fails and only the download buttons work.
- **Visitors need internet access to `cdn.jsdelivr.net`.** The page loads MathLive and MathJax from that CDN. If the server sends a Content-Security-Policy header, it must allow `script-src`, `style-src`, `font-src` and `connect-src` for `https://cdn.jsdelivr.net`, and allow `data:` and `blob:` for `img-src`. The page also uses inline `<script>` and `<style>`, so it needs `'unsafe-inline'` for both.
- No server-side runtime (Node, PHP, Python) is needed.

### Steps

1. **Decide where it lives.** Either a subdomain (for example `https://equations.pedersonlab.com/`) or a path on the main site (for example `https://pedersonlab.com/equations/`). Both work: the page uses no root-relative paths. If using a subdomain, add a DNS record (A/AAAA or CNAME) pointing it at the server first.
2. **Get the files onto the server:**
   ```bash
   sudo git clone https://github.com/danpederson83/equation-image-generator.git /var/www/equation-image-generator
   ```
3. **Point the web server at that folder.** Serve `/var/www/equation-image-generator` with `index.html` as the index file. Block the `.git` folder from being served.

   nginx, subdomain:
   ```nginx
   server {
       listen 80;
       server_name equations.pedersonlab.com;
       root /var/www/equation-image-generator;
       index index.html;
       location ~ /\.git { deny all; }
       location / { try_files $uri $uri/ =404; }
   }
   ```

   nginx, path on the existing pedersonlab.com server block:
   ```nginx
   location /equations/ {
       alias /var/www/equation-image-generator/;
       index index.html;
   }
   location ~ ^/equations/\.git { deny all; }
   ```

   Caddy, subdomain (Caddy handles HTTPS automatically):
   ```
   equations.pedersonlab.com {
       root * /var/www/equation-image-generator
       file_server
       @git path /.git/*
       respond @git 404
   }
   ```

   Apache: a `DocumentRoot` (subdomain) or `Alias /equations /var/www/equation-image-generator` (path), plus `RedirectMatch 404 /\.git`.
4. **Enable HTTPS.** With nginx or Apache, use Let's Encrypt, for example `sudo certbot --nginx -d equations.pedersonlab.com`. Caddy does this on its own.
5. **Reload the web server and check it:** open the URL, type an equation, and confirm the preview renders and "Copy image" works.

### Updating later

```bash
cd /var/www/equation-image-generator && sudo git pull
```

No restart is needed. Browsers may cache the old page, so do a hard refresh to check.

## Built with

- [MathLive](https://cortexjs.io/mathlive/) for the visual equation editor
- [MathJax](https://www.mathjax.org/) for turning equations into SVG
