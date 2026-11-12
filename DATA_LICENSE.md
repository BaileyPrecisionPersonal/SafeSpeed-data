# Data licence

## Bundled zones and cameras

`uk_zones_seed.json` (inside the app at `app/src/main/assets/`, and published at <https://github.com/BaileyPrecisionPersonal/SafeSpeed-data>) holds the average speed zones and fixed speed cameras that ship with SafeSpeed UK. It is a database derived from [OpenStreetMap](https://www.openstreetmap.org) and is **© OpenStreetMap contributors**.

OpenStreetMap data is published under the [Open Database License (ODbL) v1.0](https://opendatacommons.org/licenses/odbl/1-0/). Because this file is a derived database, it is made available under the same licence: you are free to copy, distribute, transform and build upon it, as long as you attribute OpenStreetMap, keep the result open under the ODbL, and keep it free of technical restrictions. The full terms are in the licence linked above; a plain-language summary is at <https://www.openstreetmap.org/copyright>.

The file is in SafeSpeed's backup format (`format: safespeed-zones`), described in the README under "The zone database". Each zone is one camera-to-camera section with its OpenStreetMap relation id (`osmId`) and section number (`osmPart`); each camera carries its OpenStreetMap node id. The `roadPath` of a zone is the road between its cameras, routed on OpenStreetMap data by the public OSRM servers (FOSSGIS, and the OSRM project's demo server), and is likewise derived from OpenStreetMap.

How it was made: `UkZonesSeedGenerator` (see the README, "Building") downloads `type=enforcement` + `enforcement=average_speed` relations and `highway=speed_camera` nodes for the UK from the Overpass API and writes this file.

## Backups made by the app

A backup file made with **Zones › ⋮ › Back up** contains the zones and cameras on your phone, most of which come from OpenStreetMap, so it carries the same attribution and licence. Zones and cameras you added yourself are yours.

## Other data shown in the app

- Road speed limits and road names shown while driving are fetched live from OpenStreetMap via the Overpass API: © OpenStreetMap contributors, ODbL.
- Map tiles are served by [OpenFreeMap](https://openfreemap.org) from OpenStreetMap data.
- Speed-limit signs drawn in the app follow the designs in the UK Traffic Signs Regulations, which are Crown copyright and available under the [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/).
