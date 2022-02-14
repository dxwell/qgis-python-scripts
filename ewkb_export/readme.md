# QGIS Python EWKB

This Python script exports the [Australian ABS Mesh Blocks Shapefile](https://www.abs.gov.au/statistics/standards/australian-statistical-geography-standard-asgs-edition-3/jul2021-jun2026/access-and-downloads/digital-boundary-files) to a CSV with Extended Well Known Binary (EWKB) multi-polygon geometry.

EWKB is a PostGIS extension of WKB. EWKB embeds the SRID into the geometry object.

Use this script to create a CSV for COPY into Redshift or PostgreSQL.
