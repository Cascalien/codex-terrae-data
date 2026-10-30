# Codex Terrae data

Map data that the Codex Terrae app downloads on demand, so the app itself stays small. The app checks
every file against its SHA-256 before using it.

## Altered rock (release `altered-rock-v1`)

One PMTiles file per state (`altered-rock-wa.pmtiles` ... `altered-rock-az.pmtiles`): lossless WebP
tiles, zooms 5 to 12, of eight groups of hydrothermally altered rock and key minerals read from the
ASTER satellite, for Washington, Oregon, Idaho, Montana, Nevada, Utah, Colorado and Arizona.
Checksums: `altered-rock-SHA256SUMS.txt`.

Source: Rockwell, B.W., and Bonham, L.C., 2017, Digital maps of hydrothermal alteration type, key
mineral groups, and green vegetation of the western United States derived from automated analysis of
ASTER satellite data: U.S. Geological Survey data release, https://doi.org/10.5066/F7CR5RK7 (public
domain), grouped and cleaned as described in the app's tools/aster/README.md. Farmland, towns, water and
wetlands left empty using USGS Annual NLCD 2005 land cover.

What it can't tell you: the layer is what the satellite read on bare ground. Forest, snow, cloud and
fields hide the rock, so a blank isn't "unaltered", and most altered rock holds no ore.
