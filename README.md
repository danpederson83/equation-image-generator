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
3. After about a minute the site is live at `https://<your-username>.github.io/equation-svg-generator/`. Bookmark it.

## Built with

- [MathLive](https://cortexjs.io/mathlive/) for the visual equation editor
- [MathJax](https://www.mathjax.org/) for turning equations into SVG
