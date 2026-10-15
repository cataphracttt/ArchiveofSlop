# AI Ad Archive


Live site (GitHub Pages): https://cataphracttt.github.io/ArchiveofSlop/

The site is plain static files. There is no build step.

## Files

| Path | What it is |
|---|---|
| `index.html` | v1 of the site: main page (`#/`) and index (`#/archive`). HTML, CSS and JS in one file. |
| `v2/` | Version 2 (`index.html` plus `gifs/`). See the v2 section below. |
| `v3/` | Version 3 (`index.html`). See the v3 section below. |
| `ads.json` | All the data: `companies` and `ads` (see below). Add an ad by editing this file. |
| `images/` | Full-size ad photos, WebP, longest side 1600px, quality 80. Shown in the info popup. |
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
      "note": "Optional internal note. Not shown on the site at the moment."
    }
  ]
}
```

- `id` is unique and never reused. The index sorts by `date` (newest first), so order in the file does not matter.
- `company` must match a key in `companies`. Reuse a company's key for a new ad from the same company.
- `date` is `YYYY-MM-DD`. Leave it as `""` if unknown; the ad shows as "Date unknown" and sorts last.
- `type` is `In person` or `Online`. The Type filter offers both.
- `spot` (optional) is the line or station a contributor typed, such as "F Train". `comment` (optional) is the contributor's comment. `submitted` (optional) is the Form timestamp, used to avoid adding a response twice. Every entry gets a Location row in the info popup in every version: "NY" for the archive's own entries and "NY, " plus `spot` for contributed ones (for example "NY, F train"). Entries whose `uploader` is not `Archive` also get an Uploader row. `comment` is only used by v3.
- `location`, `type` and `uploader` default to `New York`, `In person` and `Archive` if left out. `uploader` and `note` are kept in the data but are not shown or filterable on the site at the moment.
- `image` is a file name that exists in both `images/` and `thumbs/`.

## Adding an ad

1. Make two WebP files with the same name, `NN-company.webp` (`NN` is the ad's id): one with the longest side at 1600px (quality 80) into `images/`, one at 800px (quality 72) into `thumbs/`. Save without EXIF so no location data is published.
2. Add the entry to `ads` in `ads.json`, and a `companies` entry if the company is new.
3. Commit to `main`. GitHub Pages republishes in about a minute.

Photos of any shape work. Every ad sits centred and uncropped in a square box.

## v2

An alternative version of the site lives in `v2/` (https://cataphracttt.github.io/ArchiveofSlop/v2/). It shares the photos, fonts and `ads.json` with v1, so new ads appear in both. The main page of each version has a "v1. v2." switcher at the top right.

v2 differences from v1:

- Pressing "Enter the Archive" plays six gifs in fixed positions on the main page, all at once. Each plays once and holds its last frame, and the archive opens after a fixed countdown (`MAIN_WAIT` in `v2/index.html`, in milliseconds). The countdown runs even if a gif is slow to load, and it ignores the reduced-motion setting.
- Clicking an ad in the archive plays `greenscreen.gif` (576px wide on the artboard) centred on that ad, and the info popup opens after a fixed delay (`AD_WAIT`, in milliseconds). Clicks are ignored while the gif plays. Links straight to an ad (`#/archive/21`) open the popup immediately.
- The gifs are in `v2/gifs/` (`rocket.gif`, `explosion.gif`, `greenscreen.gif`). They are copies of the originals with the loop flag removed so they play once.
- Text selection is black with white text.
- Link hovers are blue (#0000FF), the same blue the "Enter the Archive" title turns on hover and while the gifs play.

## Friend link (all versions)

In v1, v2 and v3 the phrase "Think of the infamous friend ads that took over every advertisable surface not too long ago" is underlined and links to the New York Times article on archive.is (opens in a new tab). Hovering it still shows the Friend photo behind the page.

## v3

A copy of v1 (https://cataphracttt.github.io/ArchiveofSlop/v3/) with a colour mode. It shares the photos, fonts and `ads.json` with the other versions.

- Text selection is #e04918 with blue (#1818e0) text, the same saturation and lightness with the hue turned to blue. Link hovers are #e04918, except the Friend phrase, which stays black so it can be read on the colour image.
- Colour mode is off when the page loads. Clicking the Friend link opens the article and turns it on. A refresh turns it off again.
- When it is on, the main page background stays in colour until the page is refreshed (or colour mode is turned off in the popup): a #e04918 fill with the photo on top in the CSS `hue` blend mode (the same as the Illustrator HUE layer), a faint copy of the photo on top with no blend mode, and a 40px bar fixed to the bottom of the window (#e2572a) holding the credit.
- Also when it is on, hovering any tile in the archive shows that tile in the same colour effect, and the photo in the info popup is shown in colour. Clicking the photo in the popup turns colour mode off or on for the whole site.
- In the popup, while colour mode is on, a contributor's comment (if the entry has one) flies right to left across the photo on repeat, clipped to the photo, at a new random height each pass. It does not appear in v1 or v2.
- The faint copy of the photo oscillates slowly between 1% and 8% opacity. The settings are at the top of the v3 script block in `v3/index.html`:

```js
var COLOR_OVERLAY=true;        // set to false to turn the overlay off
var OV_MIN=0.01,OV_MAX=0.08;   // overlay opacity range
var OV_PERIOD=6;               // seconds for one full cycle
var FLY_SPEED=150;             // how fast a contributor's comment flies across the popup photo (artboard px per second)
```

## Contributed entries (Google Form)

Contributors submit photos through the Google Form (https://forms.gle/3X11ahuzRHz3CHpT9). Responses land in this sheet: https://docs.google.com/spreadsheets/d/1v4JCyLdqnnbKYVcHeBs7vQROEIGxiuW8IRF57SJNJZ8/edit?usp=sharing

Columns: Timestamp, Image(s) (private Google Drive links), Date Taken, Location where photo was taken (subway line/station), Name of uploader/photographer, Anything else? (comment), Do you want to be credited.

The Drive photos are private, so they cannot be fetched automatically. To add new responses: download the new photos from Drive, upload them to a Claude chat along with a request to sync the sheet, then Claude reads the sheet, matches photos to rows, identifies the company, makes the WebP files and adds entries to `ads.json`. Rules used so far:

- One tile per photo. A response with several photos becomes several entries sharing the same contributor, date, location and comment.
- Names are normalised to one spelling per person (for example "Satsuki", "satsuki yasui" and "Satsuki Yasui" are all "Satsuki Yasui"). A name of "unknown" means the contributor did not want credit.
- `location` stays "New York" so the Location filter stays tidy. The typed line or station goes in `spot`, lightly tidied: capitalisation fixed ("F train", "Court Sq G train", "34th St") and side notes such as "(the same train cart)" dropped.
- Dates are taken as typed (a missing year means 2026).

## Preview locally

```
python3 -m http.server 8000
```

Open http://localhost:8000. Opening `index.html` straight from disk will not load `ads.json`, because browsers block that.

## Design notes

- Everything is measured on a 1920 x 1080 artboard and scales with the window width (`--u` = 1/1920 of the width). Body text is 19px, the main page title is 204px.
- Body text is AR One Sans Regular. The large "Enter the Archive" title is AR One Sans Retina Regular.
- No hover animations.
- The index grid is four equal square boxes, 16px apart, starting 205px from the left and flush with the top and right edges. Every photo fills its box (cropped if needed). The left column (header, Submit, filters) stays fixed, 43px from the left.
- Filters: Date (by month, with Newest first / Oldest first sorting inside the Date list), Location and Type. Unknown dates always sort last.
- Clicking an ad opens a popup that is always centred. It shows the whole photo (longest side 620px on the artboard) beside nine rows: Company, Market, Founded, Valuation, Employees, HQ, CEO, Ad Copy and Date. The margins are 24px on the left, right and bottom, and 36px on top for the Close button. The outline is 1px, the same as the thin underline.
- The Submit links go to the Google Form: https://forms.gle/3X11ahuzRHz3CHpT9
- Company details are compiled from public reporting. Check them before citing.

## For Claude sessions

Read this file first, then `ads.json`. Pull the latest files before changing anything. Keep `index.html` free of embedded images and fonts; add new photos as WebP files as described above. Use short plain commit messages, such as "Add 6 ads from 14 Oct".
