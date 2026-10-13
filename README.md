# AI Ad Archive


Live site (GitHub Pages): https://cataphracttt.github.io/ArchiveofSlop/

The site is plain static files. There is no build step.

## Files

| Path | What it is |
|---|---|
| `index.html` | The whole site: main page (`#/`) and index (`#/archive`). HTML, CSS and JS in one file. |
| `ads.json` | All the data: `companies` and `ads` (see below). Add an ad by editing this file. |
| `images/` | Full-size ad photos, WebP, longest side 1600px, quality 80. Shown in the details sheet. |
| `thumbs/` | Grid thumbnails, WebP, longest side 800px, quality 72. Same file names as `images/`. |
| `images/friend-ads-nyt.webp` | The main page hover photo. Credit: Hiroko Masuike/The New York Times. |
| `fonts/` | AR One Sans Regular and AR One Sans Retina Regular, WOFF2. |
| `.nojekyll` | Tells GitHub Pages to serve the files as they are. |

## ads.json

```json
{
  "companies": {
    "blacksmith": {
      "name": "Blacksmith", "market": "...", "founded": "2023",
      "valuation": "...", "employees": "...", "hq": "...", "ceo": "..."
    }
  },
  "ads": [
    {
      "id": 30,
      "company": "blacksmith",
      "copy": "Run GitHub Actions 2x faster.",
      "place": "Subway station wall",
      "date": "2026-09-20",
      "location": "New York",
      "type": "In person",
      "uploader": "Archive",
      "image": "30-blacksmith.webp",
      "note": "Optional short note shown in the details sheet."
    }
  ]
}
```

- `id` is unique and never reused. The index sorts by `date` (newest first), so order in the file does not matter.
- `company` must match a key in `companies`. Reuse a company's key for a new ad from the same company.
- `date` is `YYYY-MM-DD`. Leave it as `""` if unknown; the ad shows as "Date unknown" and sorts last.
- `type` is `In person` or `Online`. The Type filter offers both.
- `location`, `type` and `uploader` default to `New York`, `In person` and `Archive` if left out.
- `image` is a file name that exists in both `images/` and `thumbs/`.

## Adding an ad

1. Make two WebP files with the same name, `NN-company.webp` (`NN` is the ad's id): one with the longest side at 1600px (quality 80) into `images/`, one at 800px (quality 72) into `thumbs/`. Save without EXIF so no location data is published.
2. Add the entry to `ads` in `ads.json`, and a `companies` entry if the company is new.
3. Commit to `main`. GitHub Pages republishes in about a minute.

Photos of any shape work. Every ad sits centred and uncropped in a square box.

## Preview locally

```
python3 -m http.server 8000
```

Open http://localhost:8000. Opening `index.html` straight from disk will not load `ads.json`, because browsers block that.

## Design notes

- Everything is measured on a 1920 x 1080 artboard and scales with the window width (`--u` = 1/1920 of the width). Body text is 19px, the main page title is 204px.
- Body text is AR One Sans Regular. The large "Enter the Archive" title is AR One Sans Retina Regular.
- No hover animations.
- The index grid is four equal square boxes, 18px apart, starting 235px from the left and flush with the top and right edges. The left column (header, Submit, filters) stays fixed.
- The Submit links go to the Google Form: https://forms.gle/3X11ahuzRHz3CHpT9
- Company details are compiled from public reporting. Check them before citing.

## For Claude sessions

Read this file first, then `ads.json`. Pull the latest files before changing anything. Keep `index.html` free of embedded images and fonts; add new photos as WebP files as described above. Use short plain commit messages, such as "Add 6 ads from 14 Oct".
