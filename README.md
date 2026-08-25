#  WFS-Transaction Editor through Geoserver

A simple, lightweight web-based mapping application that demonstrates how to view and edit geographical features using OpenLayers and Web Feature Service - Transactional (WFS-T) with GeoServer.

## Features

- **Navigate:** Pan and zoom around the map.
- **Insert Shape:** Draw new polygons on the map. The application automatically converts them to MultiPolygons (if required by your database) and assigns default attributes.
- **Update Shape:** Modify the vertices of existing features on the map.
- **Delete Shape:** Select features to remove them from the layer.
- **Save to DB:** Commits all insertions, updates, and deletions back to the backend database via a WFS-T POST request to GeoServer.

## Prerequisites

To use this editor, you will need:

1. **GeoServer:** A running instance of GeoServer (the code defaults to `http://localhost:8080/geoserver/wfs`).
2. **Published Layer:** A vector layer published in GeoServer. The default code expects:
   - Workspace/Namespace: `cite`
   - Layer Name: `pak_adm2`
3. **WFS Enabled:** Ensure that the Web Feature Service (WFS) is enabled in GeoServer and that Transactional capabilities (WFS-T) are turned on.
4. **CORS (Cross-Origin Resource Sharing):** If you are running this HTML file on a different port or domain than GeoServer, you must enable CORS in your GeoServer configuration (usually by uncommenting CORS filters in `web.xml`).

## Configuration

Before running the application, you may need to update the configuration variables inside the `<script>` tag in `working wfs-t.html` to match your GeoServer setup:

```javascript
// --- CONFIGURATION ---
const featureNS = 'http://www.opengeospatial.net/cite'; // Namespace URI from GeoServer
const layerName = 'pak_adm2'; // Your layer name
const geometryName = 'geom'; // The name of the geometry column in your database ('geom' or 'the_geom')
```

You should also update the GeoServer URL in the `wfsSource` URL function and the `saveChanges` function if your GeoServer is not hosted at `http://localhost:8080/geoserver/wfs`.

Additionally, if your database schema requires specific attributes when inserting a new feature, update the `interactDraw.on('drawend', ...)` event listener in the code to set your required attributes.

## How to Run

Since it is a standard HTML file with client-side JavaScript, you can simply open the `working wfs-t.html` file in any modern web browser. If you prefer, you can serve it using a local web server (e.g., VS Code Live Server, Python `http.server`, etc.).

## Notes

- **Geometry Name Fix:** The code includes a workaround to properly format WFS-T XML payloads when the backend database uses a geometry column name (like `geom`) that differs from the OpenLayers default (`geometry`).
- **Dependencies:** The project uses OpenLayers v7 via CDN (`https://cdn.jsdelivr.net/npm/ol@v7.4.0/`). No local `npm install` is required.
