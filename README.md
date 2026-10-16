# AI Ad Archive


Live site (GitHub Pages): https://cataphracttt.github.io/ArchiveofSlop/

The site is plain static files. There is no build step.

## Files

| Path | What it is |
|---|---|
| `index.html` | The live site (formerly v3): main page (`#/`) and index (`#/archive`). HTML, CSS and JS in one file. |
| `v1/` | Version 1 (`index.html`). Not linked from the site. Kept in the repo. |
| `v2/` | Version 2 (`index.html` plus `gifs/`). Not linked from the site. Kept in the repo. See the v2 section below. |
| `v3/` | A redirect to the homepage, so old v3 links still work. |
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

On the live site, v1 and v2 the phrase "Think of the infamous friend ads that took over every advertisable surface not too long ago" is underlined and links to the New York Times article (https://www.nytimes.com/2025/10/07/style/friend-ai-subway-ads-new-york.html, opens in a new tab). Hovering it still shows the Friend photo behind the page.

## The live site (formerly v3)

The homepage (https://cataphracttt.github.io/ArchiveofSlop/) is what used to be v3, a copy of v1 with a colour mode. v1 and v2 are still in the repo (`v1/`, `v2/`) but nothing on the site links to them. They share the photos, fonts and `ads.json`.

- Text selection is #e04918 with black text. After colour mode is triggered, selection on the main page (only) is #5963e0. Link hovers are #e04918, except the Friend phrase, which stays black so it can be read on the colour image.
- Colour mode is off when the page loads. Clicking the Friend link opens the article and turns it on. A refresh turns it off again. When it is on, the main page background stays in colour: a #e04918 fill with the photo on top in the CSS `hue` blend mode (the same as the Illustrator HUE layer), a faint copy of the photo on top with no blend mode, and a 40px bar fixed to the bottom of the window (#e2572a) holding the credit.
- In the archive, hovering any tile always shows that tile in the colour effect. In the info popup the photo shows in colour while it is hovered (a tap toggles it on touch screens), and while it is in colour a contributor's comment (if the entry has one) flies right to left across the photo on repeat, clipped to the photo, at a new random height each pass. The popup's colour state does not affect tile hovers or the main page.
- The sidebar is 43px from the left and the photos start 43px after the widest line ("AI AD ARCHIVE"), so the margins on both sides of the column are even. The filter block reads FILTER: with an underlined Clear on the same line, then Date, Location and Type at an even 36px spacing. There is no "All" option: click a selected option again to turn it off, or press Clear to reset every filter and the sort order. Date lists Newest first / Oldest first, then the months.
- Changes are animated with short (about 0.2 second) fades or slides: filters opening and closing, hover colour on tiles, the popup photo, the main page colour, the hover photo, tiles appearing, the popup opening and link colours.
- The faint copy of the photo oscillates slowly between 1% and 8% opacity. The settings are at the top of the colour-mode script block in `index.html`:

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
- Dates are taken as typed (a missing year means 2026), unless the photo's own EXIF date proves the typed date wrong (for example a date in the future). EXIF is read for checking only and is never published: the WebP files carry no EXIF or location data.
- Files downloaded from Drive are named `<camera name> - <Drive account name>`. The Drive account name can differ from the name typed in the Form (for example "Sketchy Raven" is Vincy and "Charlie Caulfield" is Lee Caulfield). The typed name is the one shown on the site.
- Photos are matched to rows using the camera file numbers (they increase with time), the EXIF dates, the comment and the ad itself.
- If the advertiser is not visible in a photo, the entry is listed with the company "Unknown" (the `unidentified-legal-ai` company in `ads.json`) until someone identifies it.
- A comment is attached only to the photos it is about (for example the cloning comment is on the two Synthesia photos, not the Parallel photo from the same submission).

## Preview locally

```
python3 -m http.server 8000
```

Open http://localhost:8000. Opening `index.html` straight from disk will not load `ads.json`, because browsers block that.

## Design notes

- Dates are abbreviated as "Oct. 2026" in the Date filter and "9 Oct. 2026" in the popup (Jan., Feb., Mar., Apr., May, Jun., Jul., Aug., Sep., Oct., Nov., Dec.).
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
