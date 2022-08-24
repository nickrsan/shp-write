# shp-write

Writes shapefile in pure javascript. Uses [dbf](https://github.com/tmcw/dbf)
for the data component, file-saver, and [jsZIP](http://stuk.github.io/jszip/) to generate
ZIP file downloads in-browser.

This fork has been modified from the original mapbox version and uses
the fixes for various geometries and zipfile downloads in https://github.com/hwbllmnn/shp-write - see
[Pull Requests Contained in this Branch](#pull-requests-contained-in-this-branch) below for more details

I have further made minor readme updates and 
packaged this for NPM so it is available immediately as a browserified package, see below.

## Usage

For node.js or [browserify](https://github.com/substack/node-browserify)

    npm install --save @nickrsan/shp-write

Or in a browser

    https://unpkg.com/@nickrsan/shp-write@latest/shpwrite.js

## Testing

To test the download functionality run `npm run make-test` and open index.html in browser.
This should start an immediate download of test features defined in `indexTest.js`.

## Caveats

* Requires a capable fancy modern browser with [Typed Arrays](http://caniuse.com/#feat=typedarrays)
  support
* Geometries: Point, LineString, Polygon, MultiLineString, MultiPolygon
* Tabular-style properties export with Shapefile's field name length limit

## Example

```js
var shpwrite = require('shp-write');

// (optional) set names for zip file, zipped folder and feature types
var options = {
    file: 'zipfilename',
    folder: 'myshapes',
    types: {
        point: 'points_shapefilename',
        polygon: 'polygon_shapefilename',
        polyline: 'lines_shapefilename'
    }
}
// a GeoJSON bridge for features
shpwrite.download({
    type: 'FeatureCollection',
    features: [
        {
            type: 'Feature',
            geometry: {
                type: 'Point',
                coordinates: [0, 0]
            },
            properties: {
                name: 'Foo'
            }
        },
        {
            type: 'Feature',
            geometry: {
                type: 'Point',
                coordinates: [0, 10]
            },
            properties: {
                name: 'Bar'
            }
        }
    ]
}, options);
// triggers a download of a zip file with shapefiles contained within.
```

## API

### `download(geojson)`

Given a [GeoJSON](http://geojson.org/) FeatureCollection as an object,
converts convertible features into Shapefiles and triggers a download.

### `write(data, geometrytype, geometries, callback)`

Given data, an array of objects for each row of data, geometry, the OGC standard
geometry type (like `POINT`), geometries, a list of geometries as bare coordinate
arrays, generate a shapfile and call the callback with `err` and an object with

```js
{
    shp: DataView(),
    shx: DataView(),
    dbf: DataView()
}
```

### `zip(geojson)`

Generate a ArrayBuffer of a zipped shapefile, dbf, and prj, from a GeoJSON
object.

## Other Implementations

* [https://code.google.com/p/pyshp/](https://code.google.com/p/pyshp/)

## Reference

* [http://www.esri.com/library/whitepapers/pdfs/shapefile.pdf](http://www.esri.com/library/whitepapers/pdfs/shapefile.pdf)

## Contributors

* Nick Baugh <niftylettuce@gmail.com>

## Pull requests contained in this branch

This branch includes the following PRs of the official repository:

* [IE download fix](https://github.com/mapbox/shp-write/pull/50) (merged manually as the source repo is gone)
* [Fix point export](https://github.com/mapbox/shp-write/pull/69)
* [Fixed extraction of polygon coordinates](https://github.com/mapbox/shp-write/pull/65)
