# QGIS Python EWKB

This Python script exports the [Australian ABS Mesh Blocks Shapefile](https://www.abs.gov.au/statistics/standards/australian-statistical-geography-standard-asgs-edition-3/jul2021-jun2026/access-and-downloads/digital-boundary-files) to a CSV with Extended Well Known Binary (EWKB) multi-polygon geometry.

EWKB is a PostGIS extension of WKB. EWKB embeds the SRID into the geometry object.

Use this script to create a CSV for COPY into Redshift or PostgreSQL.

```python
# Python script to export the abs_polygon shapefile into CSV
# Polygon geometries are exported as EWKB

# Set the output file
output_file_name = '/users/davidcrosswell/abs_output.csv'
# Set the SRID - 4283 is GDA94, 7844 is GDA2020
set_SRID = 4283

# Import libraries
from shapely import geos, wkb, wkt
# Get the active QGIS layer
layer = iface.activeLayer()
# Set the Include SRID to true
geos.WKBWriter.defaults['include_srid'] = True
# Open a file to write to
with open(output_file_name, 'w') as absfile:
    # Write the header
    absfile.writelines('MB_CODE21,MB_CAT21,CHG_FLAG21,CHG_LBL21,SA1_CODE21,SA2_CODE21,SA2_NAME21,SA3_CODE21,SA3_NAME21,SA4_CODE21,SA4_NAME21\n')
    # Write out each geometry object
    for index, f in zip(range(10), layer.getFeatures()):
    # for f in layer.getFeatures():
        # Put the WKT geometry into an object
        geom = f.geometry()
        # if geometry is empty - some abs polygon geometries are missing
        if geom.isEmpty():
            geomwkb = ""
        # if geometry is valid
        else:       
            geom_wkt = wkt.loads(geom.asWkt())
            # Set the SRID of the geom object
            geos.lgeos.GEOSSetSRID(geom_wkt._geom, set_SRID)
            # Convert to binary
            geomwkb = geom_wkt.wkb_hex
        # Construct the output: mod_ref has been replaced 
        line = '%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s' % (geomwkb, f['MB_CODE21'], f['MB_CAT21'], f['CHG_FLAG21'], f['CHG_LBL21'], f['SA1_CODE21'], f['SA2_CODE21'], f['SA2_NAME21'], f['SA3_CODE21'], f['SA3_NAME21'], f['SA4_CODE21'], f['SA4_NAME21'])
        absfile.write(line+'\n')
        
```
