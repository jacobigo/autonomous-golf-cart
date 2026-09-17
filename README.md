# autonomous-golf-cart

Project page for the autonomous golf cart MS thesis. Served at
<https://jacobigo.github.io/autonomous-golf-cart/>.

Plain static HTML + CSS, no build step. Every path is relative, so the page works
under `/autonomous-golf-cart/` and when opened locally.

```
index.html     the page
style.css      styles (light + dark via prefers-color-scheme)
assets/        web-ready media only
.nojekyll      tells Pages to serve files as-is
```

## Preview locally

```bash
python3 -m http.server 8000   # then open http://localhost:8000
```

## Adding media

**Look at every photo before committing it.** Things to check for: receipts, the
Yamaha wiring diagram on a screen or bench, a license plate or VIN, a house number or
street, and other people's faces. This repo is public, and a photo can leak what grep
won't catch. Remove location metadata as well; the `ffmpeg` commands below do that.

Keep originals in `raw/`, which is gitignored.

**Video** — GitHub rejects files over 100 MB. Aim for under ~10 MB:

```bash
ffmpeg -i raw/dbw_demo.MOV -vf "scale=-2:720" -c:v libx264 -crf 26 -preset slow \
  -an -movflags +faststart -map_metadata -1 assets/dbw_demo.mp4
ffmpeg -i assets/dbw_demo.mp4 -ss 2 -frames:v 1 -q:v 3 assets/dbw_demo-poster.jpg
```

Remove `-an` to keep sound. The page expects `assets/dbw_demo.mp4` and its poster.

**Photos** — re-encode with metadata stripped (add `-vf "scale=1600:-2"` for large originals):

```bash
ffmpeg -i raw/golfcart.jpeg -q:v 4 -map_metadata -1 assets/golfcart.jpg
```

Add each photo to the `.gallery` block in `index.html` as a `<figure>` with real `alt` text.

## Deploy

Settings → Pages → Source: *Deploy from a branch* → `main` / `(root)`.
Pushes to `main` go live in about a minute.

## Content rules

Content is written by hand for this page and never copied from the private thesis
repo. Keep it high level: no pin maps, board layouts, fault matrices, costs, or
quotes from the evidence files. Readers who want the details get them when the code
is released with the thesis.
