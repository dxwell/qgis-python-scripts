# qgis-python-scripts

## QGIS Python EWKB

This Python script exports the Australian ABS Mesh Blocks Shapefile to a CSV with Extended Well Known Binary (EWKB) multi-polygon geometry.

EWKB is a PostGIS extension of WKB. EWKB embeds the SRID into the geometry object.

Use to create a CSV for COPY into PostgreSQL or Redshit.
