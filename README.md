# mini-naru
Tile creation subset of unvt/naru that works on unvt/nanban

# Background
unvt/naru is a really good tool sets for learning the vector tile techniques.  
However, it is based on the nodejs version 12 and it cannot be built as a Docker image anymore in my local environment.   
I cannot update whole unvt/naru immediately, so I decided to extract a subset of OSM vector tile creation that would work on the unvt/nanban container.

# Usage
## Start unvt/naru (docker container)  
```
docker run -it --rm -t ${PWD}:/data unvt/nanban
cd /data
npm install
```

## Download the data (on Docker container)
```
curl -C - 'https://download.geofabrik.de/north-america/{us-northeast-latest.osm.pbf}' --output './src/#1'
```

## Vector tile conversion to mbtiles  (on Docker container) 
(edit source file name when necessary.)
```
osmium export --config osmium-export-config.json --index-type=sparse_file_array --output-format=geojsonseq --output=- src/us-northeast-latest.osm.pbf | node filter.js | tippecanoe --no-feature-limit --no-tile-size-limit --force --simplification=2 --maximum-zoom=15 --base-zoom=15 --hilbert --output=src/tiles.mbtiles
```
You might want to use clip bounding box (--clip-bounding-box=minlon,minlat,maxlon,maxlat). (It does not go well yet. I might be wrong at some place.)
```
osmium export --config osmium-export-config.json --index-type=sparse_file_array --output-format=geojsonseq --output=- src/us-northeast-latest.osm.pbf | node filter.js | tippecanoe --no-feature-limit --no-tile-size-limit --force --simplification=2 --clip-bounding-box=40,-76,42,-72 --maximum-zoom=15 --base-zoom=15 --hilbert --output=src/tiles.mbtiles
```


## ZXY tile creation from mbtiles (on Docker container)
```
tile-join --force --no-tile-compression --output-to-directory=docs/zxy --no-tile-size-limit src/tiles.mbtiles
```

## styling with unvt/charites (on Docker container)
after editing a series of yaml files.
```
charites build style/style.yml docs/style.json
```

# License 
The license follows the license of unvt/naru

# Reference
unvt/naru https://github.com/unvt/naru