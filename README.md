# Offline place data for Local Photos AI

Prebuilt map data that the **Local Photos AI** photo-album app downloads
**once, during setup**, so it can name photos from their GPS coordinates
entirely offline, e.g. *"Near Maple Pass, Okanogan-Wenatchee National Forest,
Washington, United States"*.

This repository holds **data only**: no application code. The files are
published as [GitHub Releases](../../releases); `geodata_manifest.json` lists
the latest release's files, sizes and SHA-256 checksums.

## What's in a release

| File | Contents | Size (gzip) |
|---|---|---|
| `places-world.sqlite.gz` | GeoNames towns and villages, passes, peaks, lakes, waterfalls, glaciers, parks and landmarks worldwide (~6.9 million places) | ~305 MB |
| `places-<CC>.sqlite.gz` | The same, one file per country (ISO code), for installs that only want some countries | 1 KB – 37 MB each |
| `padus.sqlite.gz` | Boundaries of US national/state/local parks, national forests, wilderness, wildlife refuges and monuments | ~51 MB |

Each file is a gzipped SQLite database with R*Tree spatial indexes.
Files over 20 MB are published as numbered pieces (`places-world.sqlite.gz.part01`,
`.part02`, …) that join, in order, into the file named in the manifest. Setup
downloads and checks each piece, then verifies the joined file's checksum.

## How it's used

The app's setup script picks whole world, a region or chosen countries, plus
the US parks file if the selection includes the United States. It downloads
the files from the release named in its own copy of the manifest, verifies
every checksum, and installs them locally. The app never downloads anything
at runtime. `setup --refresh-geodata` reads `geodata_manifest.json` from this
repository to pick up newer data.

The files are built with the maintainer tools in the app repository
(`tools/geodata/`) and refreshed about twice a year.

## Licences and attribution

- **GeoNames**: place data © [GeoNames](https://www.geonames.org/), licensed
  under [Creative Commons Attribution 4.0](https://creativecommons.org/licenses/by/4.0/).
  The files here are a filtered, re-indexed subset of the GeoNames
  `allCountries` dump: only the feature types used for naming photos,
  historical places removed, and admin names resolved.
- **PAD-US**: U.S. Geological Survey (USGS) Gap Analysis Project (GAP), 2024,
  *Protected Areas Database of the United States (PAD-US) 4.1*,
  <https://doi.org/10.5066/P96WBCHS>. Public domain (U.S. Government work).
  The file here is a simplified subset: named parks, forests, wilderness,
  refuges and similar areas, with boundaries simplified for fast lookups.

You may reuse these files under the same terms: credit GeoNames (CC BY 4.0)
and USGS PAD-US.
