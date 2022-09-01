# mini-naru
Tile creation subset of unvt/naru that works on unvt/nanban

# Background
unvt/naru is a really good tool sets for learning the vector tile techniques.  
However, it is based on the nodejs version 12 and it cannot built as a Docker image anymore in my local environment.  
I decided to extract a subset of vector tile creation from OSM that would work on the unvt/nanban container.

# Usage
## Start unvt/naru (docker container)  
```
docker run -it --rm -t ${PWD}:/data
cd /data
npm install
```

## Download the data
```
curl -C 'https://download.geofabrik.de/north-america/us-northeast-latest.osm.pbf' --outout './src/#1'
```

## Vector tile conversion to mbtiles   
(edit source file name when necessary.)
```
osmium export --config osmium-export-config.json --index-type=sparse_file_array --output-format=geojsonseq --output=- src/us-northeast-latest.osm.pbf | node filter.js | tippecanoe --no-feature-limit --no-tile-size-limit --force --simplification=2 --maximum-zoom=15 --base-zoom=15 --hilbert --output=src/tiles.mbtiles
```

## ZXY tile creation from mbtiles
```
tile-join --force --no-tile-compression --output-to-directory=docs/zxy --no-tile-size-limit src/tiles.mbtiles
```

# Reference
unvt/naru https://github.com/unvt/naru