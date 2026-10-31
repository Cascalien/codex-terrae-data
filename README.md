# Codex Terrae data

Map data that the Codex Terrae app downloads on demand, so the app itself stays small. The app checks
every file against its SHA-256 before using it.

## Altered rock (`altered-rock/`, v1)

One PMTiles file per state (`altered-rock-wa.pmtiles` ... `altered-rock-az.pmtiles`): lossless WebP
tiles, zooms 5 to 12, of eight groups of hydrothermally altered rock and key minerals read from the
ASTER satellite, for Washington, Oregon, Idaho, Montana, Nevada, Utah, Colorado and Arizona.
Checksums: `altered-rock/SHA256SUMS.txt`. The app reads each file at the commit that added it, so what it
downloads never changes under it (v1: commit 61415ba903fd4b4868083327e5f396b58a3f9067):
`https://raw.githubusercontent.com/Cascalien/codex-terrae-data/61415ba903fd4b4868083327e5f396b58a3f9067/altered-rock/altered-rock-wa.pmtiles`.
A new version goes in as new files and a new commit; old commits stay readable.

v1.1 (2026-10-31): nine more states from the same release, each read by the app at the commit that added it:
California, Wyoming, New Mexico, North Dakota, South Dakota, Nebraska, and the western edges of Texas, Oklahoma and
Kansas (`altered-rock-ca.pmtiles` ... `altered-rock-ks.pmtiles`). The release's western data ends partway across the
Plains: about 62% of North Dakota, 44% of South Dakota, 25% of Nebraska, and 3 to 5% of Texas, Oklahoma and Kansas are
covered. Where the northwest and southwest files overlap, the second file fills only gaps more than about 1 km from any
reading in the first (the app's tools/aster/README.md, "Per-state files").

Source: Rockwell, B.W., and Bonham, L.C., 2017, Digital maps of hydrothermal alteration type, key
mineral groups, and green vegetation of the western United States derived from automated analysis of
ASTER satellite data: U.S. Geological Survey data release, https://doi.org/10.5066/F7CR5RK7 (public
domain), grouped and cleaned as described in the app's tools/aster/README.md. Farmland, towns, water and
wetlands left empty using USGS Annual NLCD 2005 land cover.

What it can't tell you: the layer is what the satellite read on bare ground. Forest, snow, cloud and
fields hide the rock, so a blank isn't "unaltered", and most altered rock holds no ore.

## Place names (`places/`, v1)

One SQLite file per state (`places-ca.db` ... `places-tx.db`) of USGS Geographic Names Information System
(GNIS) domestic names: summits, ridges, lakes, streams, springs, flats, towns and the rest, with census, civil
and military names left out, for the app's offline place search. Same layout as the place names built into the
app (tools/places/build_places_db.py): `place(id, name, class, county, state, lat, lon)` with positions as
degrees times 100,000, and `word(w, place)` for prefix search. States: California, Wyoming, New Mexico, North
Dakota, South Dakota, Nebraska, Kansas, Oklahoma and Texas. Checksums: `places/SHA256SUMS.txt`. As with altered
rock, the app reads each file at the commit that added it.

Source: U.S. Geological Survey, Geographic Names Information System, Domestic Names, state files from
https://prd-tnm.s3.amazonaws.com/StagedProducts/GeographicNames/DomesticNames/ (public domain), downloaded
2026-10-30, built with the app's tools/places/build_place_packs.py.
