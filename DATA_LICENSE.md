# Data licence

## Zones and cameras from OpenStreetMap

Two files:
- `uk_zones_seed.json` is the full copy of the UK's zones and cameras in OpenStreetMap, as of its last refresh. It isn't in the app.
- `extras_db.json`, the Extras DB, is taken from it and built into the app as it is, unencrypted. It holds OpenStreetMap's UK zones and cameras and only ever grows: each update adds what's new in OpenStreetMap and changes nothing already there. Where the app's own camera database (below) has a zone or camera, the app leaves OpenStreetMap's copy out as it installs the Extras DB on the phone.

Both are published at <https://github.com/BaileyPrecisionPersonal/SafeSpeed-data>. Both are databases derived from [OpenStreetMap](https://www.openstreetmap.org) and are **© OpenStreetMap contributors**. The Extras DB is a separate file: nothing in it is put into the private camera database below, and nothing of that database is in it.

OpenStreetMap data is published under the [Open Database License (ODbL) v1.0](https://opendatacommons.org/licenses/odbl/1-0/). Because these files are derived databases, they are made available under the same licence: you are free to copy, distribute, transform and build upon them, as long as you attribute OpenStreetMap, keep the result open under the ODbL, and keep it free of technical restrictions. The full terms are in the licence linked above; a plain-language summary is at <https://www.openstreetmap.org/copyright>.

Both files are in SafeSpeed's backup format (`format: safespeed-zones`), described in the README under "The zone database". Each zone is one camera-to-camera section with its OpenStreetMap relation id (`sourceId`) and section number (`sourcePart`); each camera carries its OpenStreetMap node id. The `roadPath` of a zone is the road between its cameras, routed on OpenStreetMap data by the public OSRM servers (FOSSGIS, and the OSRM project's demo server), and is likewise derived from OpenStreetMap.

How it was made: `UkZonesSeedGenerator` (see the README, "Building") downloads `type=enforcement` + `enforcement=average_speed` relations and `highway=speed_camera` nodes for the UK from the Overpass API and writes the full copy. The download areas also take in a few cameras across the Channel and the Irish border; those are left out, so it holds the UK's zones and cameras only, and their limits are put in mph, as under "Built-in road speed limits" below. The Extras DB is then taken from it: what it already holds, kept as it is, and the full copy's zones and cameras it hasn't got yet (the same zone or camera is told apart as the app does it, `CameraDbCoverage`, so a route OpenStreetMap renumbered isn't added twice). It's made from OpenStreetMap alone.

## Bundled camera database (private)

`uk_camera_db_seed.json` (kept at `app/private-data/` in the app repository, and built into the app encrypted) holds the UK fixed speed cameras and average speed zones converted from the app author's own camera database. It is **not** derived from OpenStreetMap and is **not** published: it is private data for this app's own use, not covered by the ODbL and not for redistribution. The app repository that holds it is private, the public SafeSpeed-data repository never receives it (`tools/publish-data.ps1` refuses to copy it), and backups made by the app leave its zones and cameras out.

In Great Britain, each zone's `roadPath` (the road between its cameras) and its name come from Ordnance Survey's OS Open Roads: **Contains OS data © Crown copyright and database rights 2026**, under the [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/). In Northern Ireland, which OS Open Roads doesn't cover, each zone's `roadPath` comes from Land & Property Services' OSNI 50K Transport Lines: **Contains LPS Intellectual Property © Crown copyright and database right 2026**, licensed under the terms of the Open Government Licence. Until November 2026 the roads and names came from OpenStreetMap, by the public OSRM routing servers. Two things in the file still come from OpenStreetMap: whether each zone is timed one way or both, which was settled by comparing OpenStreetMap's roads at its cameras, and the names of its 16 zones in Northern Ireland (OSNI's lines have none). Those parts are © OpenStreetMap contributors. Its speed limits are the author's database's own: where it has none, the app takes the limit of the road from the built-in road file (below) on the phone, and nothing of that goes into this file (until November 2026 the file took OpenStreetMap's limit for 189 cameras and 1 zone). The file is used privately and is never made available to the public, so no public use of a derivative database under the ODbL takes place.

The database is the app's main source of cameras and zones. The Extras DB, a separate file, fills in the cameras and zones from OpenStreetMap that it doesn't have; the two are only put together on the phone.

## Built-in road speed limits

`uk_roads.rdb` (built into the app) holds every road in the UK a car drives along, with its speed limit, number, name and one-way direction, so the app can show the limit of the road you're on without going online. It is a database derived from OpenStreetMap and is **© OpenStreetMap contributors**, under the ODbL.

How it was made: `UkRoadsGenerator` reads Geofabrik's extract of the UK (`united-kingdom-latest.osm.pbf`, from <https://download.geofabrik.de/europe/united-kingdom.html>):
- It keeps the ways tagged `highway` = motorway, trunk, primary, secondary, tertiary, unclassified, residential or living_street, and the `_link` roads of the first five.
- On the island of Ireland it keeps only those inside OpenStreetMap's boundary of Northern Ireland.
- It reads each road's limit from its `maxspeed` tags, and puts every limit in mph, as UK limits are signed: a number given in km/h that is a UK limit becomes that many mph, and any other km/h limit the nearest UK limit. A limit tagged as the national speed limit (`national`, or a GB national-limit code in `maxspeed`, `maxspeed:type` or `source:maxspeed`) is marked as such. The README, "The road speed limits file", has the details.
- A road with no limit tagged is given the limit the law sets for that kind of road where it is, worked out from OpenStreetMap too: 70 on a motorway; 30 on a `residential` or `living_street` road, or one tagged `lit=yes`; the national speed limit (60, or 70 on a one-way carriageway) on one tagged `lit=no`; otherwise 30 where the road is within about 60 m of an area tagged `landuse` = residential, commercial, retail or industrial, of a residential street, or of 4 or more `building` ways, and the national speed limit elsewhere. A road that runs in and out of such places is split where it does; a stretch shorter than 200 m between two of the other kind goes with them; and a short stretch that only joins roads of 30 or less, or is a dead end off one, is taken as 30. Inside OpenStreetMap's boundary of Wales, 30 given this way is 20 instead (Wales's default on roads with street lights). A tagged limit is never changed.
- It joins pieces of road that carry on into one another with the same type, number, name, limit and direction, and simplifies each line to within 3 m.

Nothing else is added to it.

## Backups made by the app

A backup file made with **Zones › ⋮ › Back up** contains the zones and cameras on your phone, most of which come from OpenStreetMap, so it carries the same attribution and licence. Zones and cameras from the bundled camera database are private data, as above, and are not written to backups. Zones and cameras you added yourself are yours.

## Other data shown in the app

- Road speed limits and road names shown while driving come from the built-in road file above: © OpenStreetMap contributors, ODbL.
- Map tiles are served by [OpenFreeMap](https://openfreemap.org) from OpenStreetMap data.
- Speed-limit signs drawn in the app follow the designs in the UK Traffic Signs Regulations, which are Crown copyright and available under the [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/).
