# RVC Map Tile Downloader

A standalone web page for downloading map images for offline use by the Phase 3 Swarm
rovers. Enter a latitude, longitude, zoom and grid size, preview the map, and download it
as a PNG that RVC can read.

Adapted from `Rover_Website_UI/scripts/html_files/map_download.html`, rebuilt to run
anywhere — no robot, no ROS, no rosbridge.

## Using it

1. **Latitude / Longitude** — the centre of the map. Read them from RVC.
2. **Zoom (0–19)** — bigger number, more detail, smaller area. 18 suits a test site.
3. **Grid (odd, 1–11)** — how many map squares wide and tall; your point sits in the middle.
4. **API key** — see below. Pasted once, then remembered in the browser.
5. **Preview**, then **Download Image**.

### Do not rename the downloaded file

RVC parses the latitude, longitude, zoom and grid size out of the file name:

```
tiles_lat<LAT>_lon<LON>_z<ZOOM>_g<GRID>.png     (dots in lat/lon become underscores)
```

This is byte-for-byte identical to the original page's output.

## Tile source and the API key

The public OpenStreetMap tile server (`tile.openstreetmap.org`, used by the original page)
does not permit downloading tiles for offline use — see the OSMF Tile Usage Policy. This
version therefore fetches tiles from a commercial provider, configured at the top of the
script in `index.html`:

```js
const TILE_URL = 'https://api.maptiler.com/maps/streets-v2/256/{z}/{x}/{y}.png?key={key}';
```

Change that line to switch provider or map style. Confirm with your provider that your plan
permits **saving rendered tiles as image files for offline use** — this is restricted on
many plans.

### About the key

The key is entered in the page and stored in that browser's `localStorage`. Because tiles
load via `<img>` tags, the key travels in the URL query string and is therefore visible to
anyone using the site — this is unavoidable for browser-side map tiles.

Protect it by **restricting the key to your deployed domain** in the provider's dashboard
and setting a usage cap. To have operators avoid entering a key at all, hardcode it into
`TILE_URL` and delete the API key field.

## Attribution

`drawCredit()` paints `© MapTiler © OpenStreetMap contributors` into the bottom-right corner
of every downloaded image, so the credit travels with the file to the robots. OSM's licence
requires attribution on static images, not just on the web page. Update the `CREDIT`
constant if you change provider.

## Run locally

```bash
python3 -m http.server 8080
# open http://localhost:8080
```

Or just open `index.html` directly in a browser.

## Deploy

Plain static site — no build step, no framework, no dependencies.

Hosted with GitHub Pages: **Settings → Pages → Source: Deploy from a branch → `main` /
`root`**. Every push to `main` redeploys. Pages caches aggressively, so hard-refresh
(Ctrl+Shift+R) after an update.

All asset paths are relative, so the site works from the `/RVC_Map_Downloader/` subpath.

After the first deploy, restrict the tile provider API key to the deployed domain in the
provider's dashboard and set a usage cap — the key is readable by anyone using the site.

## Files

| File | Purpose |
|---|---|
| `index.html` | The page: markup, tile maths, download logic |
| `style.css` | Dark/light themes |
| `Rover_logo.png` | Navbar logo |
