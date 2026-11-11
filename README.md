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

v1.2 (2026-10-31): Idaho rebuilt the same way, adding the strip along the Nevada and Utah line (42.0 to 42.46 N, about
9,700 km²) that only the southwest file covers; none of v1's Idaho pixels changed. The app reads it at its own commit.

v1.3 (2026-10-31): Oregon rebuilt the same way, filling its strip along the California and Nevada line (42.0 to 42.26
N); none of v1's Oregon pixels changed.

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
Dakota, South Dakota, Nebraska, Kansas, Oklahoma and Texas (v1), North Carolina (v1.1, 2026-10-31), Minnesota, Iowa,
Missouri, Arkansas and Louisiana (v1.2, 2026-11-06), Wisconsin, Illinois, Michigan, Indiana, Ohio, Tennessee,
Mississippi and Alabama (v1.3, 2026-11-06), and Georgia, Florida, South Carolina, Virginia, West Virginia, Maryland,
Delaware and Pennsylvania (v1.4, 2026-11-06). Checksums: `places/SHA256SUMS.txt`. As with altered
rock, the app reads each file at the commit that added it.

Source: U.S. Geological Survey, Geographic Names Information System, Domestic Names, state files from
https://prd-tnm.s3.amazonaws.com/StagedProducts/GeographicNames/DomesticNames/ (public domain), downloaded
2026-10-30 (North Carolina's on 2026-10-31; Minnesota's, Iowa's, Missouri's, Arkansas's and Louisiana's on 2026-11-06;
v1.3's and v1.4's eight states' on 2026-11-06), built with the app's tools/places/build_place_packs.py.

v1.5 (2026-11-06): New Jersey, New York, Connecticut, Rhode Island, Massachusetts, Vermont, New Hampshire and Maine
(`places-nj.db` ... `places-me.db`), from GNIS state files downloaded 2026-11-06 and built the same way, census, civil
and military names left out. Their checksums are appended to `places/SHA256SUMS.txt`; the app reads them at the commit
that added them. GNIS puts the Saint Lawrence River's point at its mouth in the Gulf of St. Lawrence (49.67 N, 64.50 W);
the app's box for New York, which only decides where search offers the file, is drawn round the state's other names.

## Topo-map pits and shafts (`topomines/`, v1)

One zip per state (`topomines-al.zip` ... `topomines-wy.zip`), every state but North Carolina, which is built into the
app: the prospect pits, shafts, adits, quarries, open pits, sand, gravel and clay pits, tailings, dumps and disturbed
ground that USGS digitized from its 7.5- and 15-minute topographic maps (1886 to 2006). Each zip holds the state's
pins in quarter-degree files (`nv_152_w469.json`, laid out like the app's bundled North Carolina files) and
`index.json` listing each file's box and pin count, so the app reads only the files around the view. A polygon on a
state line is in both states' files (the app shows it once). Checksums: `topomines/SHA256SUMS.txt`. Built with the
app's tools/topomines/build_topo_mines.py (`pack`), written the same way every time, so a rebuild of the same data
has the same SHA-256.

Source: Horton, J.D., and San Juan, C.A., 2025, Consolidated prospect- and mine-related features from U.S. Geological
Survey 7.5- and 15-minute topographic quadrangle maps of the United States: U.S. Geological Survey data release,
https://doi.org/10.5066/P9TAALR6 (CC0 1.0: "This work is marked with the Creative Commons Zero v1.0 Universal (CC0-1.0)
public domain dedication"), read from the release's feature service on 2026-11-06.

What it can't tell you: a symbol on an old map shows where something was dug or mapped by that map's year. It isn't a
find, isn't permission to collect, and doesn't say a working is open or safe; many have been filled, built over or
mined away. Stay out of shafts and adits.

## Uranium samples (`uranium/`, v1)

One zip per state (`uranium-ak.zip` ... `uranium-wy.zip`), every state but North Carolina, which is built into the app:
USGS National Geochemical Database samples with uranium measured in them (a positive result by at least one lab method),
with thorium where the same sample has it. Each zip holds the state's samples in one-degree files (`ut_38_w110.json`)
and `index.json` listing each file's box and sample count. Locations outside the state (seafloor and lake-bottom samples)
are left out. Checksums: `uranium/SHA256SUMS.txt`. Built with the app's tools/prospects/samples/build_samples.py
(`pack-uranium`), as described in its NOTES.md, "Uranium in every state".

Source: Granitto, M., Goldman, M.A., Labay, K.A., San Juan, C.A., Azain, J.S., and Rozinek, T.R., 2026, Geochemical data
for rock, sediment, soil, and mineral samples, United States and territories, 1962 to 2023: U.S. Geological Survey data
release, https://doi.org/10.5066/P1FXE9GK (CC0 1.0), read from its feature service on 2026-11-06.

What it can't tell you: a result is what one lab found in one sample at the location recorded for it; old locations can
be rough. A high value is a place to look, not a find, and uranium and thorium minerals are radioactive.

Uranium v1.1 (2026-11-08): the same samples, files and boxes, with how each was measured. Every sample now carries "am",
the USGS method code of the uranium result shown, and "ao", its other uranium results as "value code" (a negative value is
a result under that method's lower limit, which USGS writes as minus the limit), read from the same feature service on
2026-11-08 (U_AM and U_ppm_ALL). Added with the app's tools/prospects/samples/add_uranium_methods.py; the app reads v1.1 at
the commit that added it, and v1's commit stays readable.

Place names v1.6 (2026-11-06): Alaska (`places-ak.db`, 28,986 places) and Hawaii (`places-hi.db`, 5,785 places), from
the GNIS state files downloaded 2026-11-06 and built with tools/places/build_place_packs.py as for the others. Alaska's
places cross the 180th meridian (the western Aleutians, 172 to 180 E), so the app's box for it runs from 172.44 E east
across the meridian to 130.0 W; Hawaii's reaches Kure Atoll (178.34 W) in the Northwestern Hawaiian Islands.

## Kentucky fossil localities (`fossils/`): in the app since 0.9.2.90 (Prax's decision, 2026-11-11), NOT FOR THE APP YET

Branch `kgs-fossil-points` only. The app doesn't read this file and no build ships it.

`fossils/kgs-fossils-ky.db`: the Kentucky Geological Survey's 1:24,000 Fossil Points, 3,435 fossil localities marked
on KGS / USGS 7.5-minute geologic quadrangle maps, in the same `site` table layout as the app's bundled `fossils.db`
(ids 9,000,000 + KGS's OBJECTID). Checksum: `fossils/SHA256SUMS.txt`. Built with the app's
tools/fossils/fetch_kgs.py and build_kgs_fossils.py (the app's tools/fossils/README.md, "Kentucky Geological Survey
fossil points").

Source: Kentucky Geological Survey, Geospatial Analysis Section, Digital Mapping Team, compiled by Thomas N. Sparks
and Matt M. Crawford, published by the Kentucky Division of Geographic Information on ArcGIS Online
(https://www.arcgis.com/home/item.html?id=f75962edb8b74e199954d7c00e2ee4e2), downloaded 2026-11-11.

License, read 2026-11-11, contradictory: the item's license is CC0 ("https://creativecommons.org/publicdomain/zero/1.0/";
the open data portal lists it as "CC0-1.0"), but the same terms say "These data may not be distributed outside of
Kentucky State Government. Refer inquiries to the Kentucky Geological Survey." KGS was asked by email on 2026-11-09;
the message hasn't been delivered. Don't merge until Prax decides.
