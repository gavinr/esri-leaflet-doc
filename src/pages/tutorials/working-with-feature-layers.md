---
title: Working with feature layers
description: Learn how to use feature layers in Leaflet.
layout: tutorials.hbs
---

# {{ page.data.title }}

{{ page.data.description }}

This tutorial assumes you know how to create a basic Leaflet map.  To learn how to do this, please visit the [Create your first app](https://esri.github.io/esri-leaflet/tutorials/create-your-first-app.html "Create your first app") tutorial.

##### Copy Starter HTML Page

Start by copying this skeleton code which outlines the structure of a single page website and save it in your own new `.html` file.  The code includes a `<script>` that constructs the Leaflet map object and adds a basemap from ArcGIS Online.

```xml
<!doctype html>
<html lang="en">
<head>  
  <meta charset="utf-8">
  <title>Leaflet Map with a Feature Layer</title>  
  <meta name="viewport" content="initial-scale=1,maximum-scale=1,user-scalable=no" />

  <!-- Load Leaflet from CDN-->
  <link rel="stylesheet" href="https://unpkg.com/leaflet@{{siteData.latest_leaflet}}/dist/leaflet.css"
    integrity="{{siteData.latest_leaflet_css_integrity}}"
    crossorigin=""/>
  <script src="https://unpkg.com/leaflet@{{siteData.latest_leaflet}}/dist/leaflet.js"
    integrity="{{siteData.latest_leaflet_integrity}}"
    crossorigin=""></script>  

  <!-- Load Esri Leaflet from CDN -->
  <script src="/esri-leaflet-debug.js"
    
    crossorigin=""></script>

  <style>
    html,
    body,
    #map {
      height: 100%;
      width: 100%;
      margin: 0;
      padding: 0;
    }
  </style>
</head>
<body>
  <div id="map"></div>

  <script>
    const map = L.map('map', {
      center: [45.526, -122.667],
      zoom: 13
    });

    L.esri.basemapLayer('Streets').addTo(map);
  </script>
</body>
</html>
```

##### Add a Feature Layer to the Map

We will add a REST service using the `L.esri.featureLayer` class from the Esri Leaflet plugin.  This can be either a **Map Service** or **Feature Service**.  You must provide the url for the service.  You can learn more about map and feature services at the [Introduction to Layer Types](https://esri.github.io/esri-leaflet/tutorials/introduction-to-layer-types.html "Introduction to Layer Types Tutorial") tutorial.

Below is an example for creating a Feature Layer using both types of services:

```js
const portlandHeritageTrees = L.esri.featureLayer({ url: 'https://www.portlandmaps.com/arcgis/rest/services/Public/Parks_Misc/MapServer/21/' });

const worldCities = L.esri.featureLayer({ url: 'https://sampleserver6.arcgisonline.com/arcgis/rest/services/SampleWorldCities/MapServer/0' });
```

In order to display our new layer, we need to add it to our map using Leaflet's `addTo()` method.  We can chain this method directly to the object constructor, or call it in a separate line.  In our sample, we'll use the chaining approach.

```js
const portlandHeritageTrees = L.esri.featureLayer({
  url: 'https://www.portlandmaps.com/arcgis/rest/services/Public/Parks_Misc/MapServer/21/'
}).addTo(map);
```

If we save and refresh our map, we should see many blue markers on the map, which are the features from the service.  Leaflet is applying a default style to the features.  It is up to you to define the styles for the data tha that is drawn. See the links below for examples of doing this:

> [Styling Points](https://esri.github.io/esri-leaflet/examples/styling-feature-layer-points.html "Styling Points Feature Layer")

> [Styling Lines](https://esri.github.io/esri-leaflet/examples/styling-feature-layer-polylines.html "Styling Lines Feature Layer")

> [Styling Polygons](https://esri.github.io/esri-leaflet/examples/styling-feature-layer-polygons.html "Styling Polygon Feature Layer")


It's helpful to cross reference help documents to double-check syntax and learn more about what is possible.  Check out the [Esri Leaflet documentation](https://esri.github.io/esri-leaflet/api-reference/layers/feature-layer.html "Esri Feature Layer") for more information about additional feature layer constructor options.

##### Add Esri Leaflet Renderers Plugin

If we want to use the symbology set when a service was published, we'll need to add the [Esri Leaflet Renderers](https://github.com/Esri/esri-leaflet-renderers "Esri Leaflet Renderers") helper library.  We'll add a `<script>` for this plugin in the `<head>` element on the page.

```xml
<!-- Load Esri Leaflet Renderers from CDN -->
<script src="https://unpkg.com/esri-leaflet-renderers@{{siteData.latest_esri_leaflet_renderers}}/dist/esri-leaflet-renderers.js"
  integrity="{{siteData.latest_esri_leaflet_renderers_integrity}}"
  crossorigin=""></script>
```  

If we save and refresh our map, we'll now see the trees displayed as red circles.  

##### Add a Popup to Feature Layer

Now we will add a popup to our feature layer.  The popup can contain both static text and dynamic text from the fields in the service.  We'll use the `bindPopup` method to create the popup.  You can learn more about popups in Leaflet by visting the [documentation](https://leafletjs.com/reference.html#popup "Leaflet Popup Documentation").

We will create the content for the popup using Leaflet's [Utility Template](https://leafletjs.com/reference.html#util-template 'Leaflet Utility Template').  We place the name of fields from the service in  curly brackets **{ }**, and add `feature.properties` at the end of the `L.Util.template` to access the fields.

```js
portlandHeritageTrees.bindPopup(function (evt) {
  return L.Util.template('<h3>{COMMON_NAM}</h3><hr /><p>This tree is located at {ADDRESS} and its scientific name is {SCIENTIFIC}.', evt.feature.properties);
});
```

If we save and refresh our map, we are able to click on the features and a popup will open.

##### Final HTML File

Here is what the final `html` file should look like for this tutorial:

```xml
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Leaflet Map with a Feature Layer</title>  
  <meta name="viewport" content="initial-scale=1,maximum-scale=1,user-scalable=no" />

  <!-- Load Leaflet from CDN-->
   <link rel="stylesheet" href="https://unpkg.com/leaflet@{{siteData.latest_leaflet}}/dist/leaflet.css"
    integrity="{{siteData.latest_leaflet_css_integrity}}"
    crossorigin=""/>
  <script src="https://unpkg.com/leaflet@{{siteData.latest_leaflet}}/dist/leaflet.js"
    integrity="{{siteData.latest_leaflet_integrity}}"
    crossorigin=""></script>  

  <!-- Load Esri Leaflet from CDN -->
  <script src="/esri-leaflet-debug.js"
    
    crossorigin=""></script>
  
  <!-- Load Esri Leaflet Renderers from CDN -->
  <script src="https://unpkg.com/esri-leaflet-renderers@{{siteData.latest_esri_leaflet_renderers}}/dist/esri-leaflet-renderers.js"
    integrity="{{siteData.latest_esri_leaflet_renderers_integrity}}"
    crossorigin=""></script>

  <style>
    html,
    body,
    #map {
      height: 100%;
      width: 100%;
      margin: 0;
      padding: 0;
    }
  </style>
</head>
<body>
  <div id="map"></div>

  <script>
    const map = L.map('map', {
      center: [45.526, -122.667],
      zoom: 13
    });

    L.esri.basemapLayer('Streets').addTo(map);

    const portlandHeritageTrees = L.esri.featureLayer({
      url: 'https://www.portlandmaps.com/arcgis/rest/services/Public/Parks_Misc/MapServer/21/'
    }).addTo(map);

    portlandHeritageTrees.bindPopup(function (evt) {
      return L.Util.template('<h3>{COMMON_NAM}</h3><hr /><p>This tree is located at {ADDRESS} and its scientific name is {SCIENTIFIC}.', evt.feature.properties);
    });
  </script>
</body>
</html>
```
