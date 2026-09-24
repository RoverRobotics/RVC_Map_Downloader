# RVC Map Tile Downloader

A standalone web page for downloading map images for offline use by the Phase 3 Swarm
rovers. Enter a latitude, longitude, zoom and grid size, preview the map, and download it
as a PNG that RVC can read.

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
Web UI Link:
[RVC Map Downloader](https://roverrobotics.github.io/RVC_Map_Downloader/)

