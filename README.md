# mini-naru
Tile creation subset of unvt/naru that works on unvt/nanban

# Usage
Start unvt/naru (docker container)  
```
docker run -it --rm -t ${PWD}:/data
cd /data
npm install
```

Download the data
```
curl -C 'https://download.geofabrik.de/north-america/us-northeast-latest.osm.pbf}' --outout './src/#1'
```

Vector tile conversion to mbtiles   
(edit source file name when necessary.)
```
osmium export --config osmium-export-config.json --index-type=sparse_file_array --output-format=geojsonseq --output=- src/us-northeast-latest.osm.pbf | node filter.js | tippecanoe --no-feature-limit --no-tile-size-limit --force --simplification=2 --maximum-zoom=15 --base-zoom=15 --hilbert --output=src/tiles.mbtiles
```


```
tile-join --force --no-tile-compression --output-to-directory=docs/zxy --no-tile-size-limit src/tiles.mbtiles
```