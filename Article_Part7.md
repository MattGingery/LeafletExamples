# Leaflet Maps – JavaScript Library - Part 7 – REST and GeoJSON

Part 7 – REST and GeoJSON Example links: [Preview](https://mattgingery.github.io/LeafletExamples/Leaflet_part7_rest_GeoJSON.htm) | [Preview Example 2](https://mattgingery.github.io/LeafletExamples/Leaflet_part7_rest_GeoJSON_more.htm) | [GitHub](https://mattgingery.github.io/LeafletExamples/Leaflet_part7_rest_GeoJSON.htm) | [GitHub Example 2](https://mattgingery.github.io/LeafletExamples/Leaflet_part7_rest_GeoJSON_more.htm) | [JSFiddle](https://jsfiddle.net/mgingery/Lm5ouvx0/)

## Introduction

In my previous examples, I showed you how to create a Leaflet interactive map using Well Known Text or other geographical text strings that can be produced by the middle tier of your web application.  This example shows you a new template for consuming existing web services that serve GeoJSON data such as those hosted on ArcGIS servers. 

## Using Web Services

In this template, I created a “config” JavaScript object that contains the kinds of configuration details you are likely to customize such as a map center coordinates, starting zoom level, URL of the REST web service, columns to display, and (optionally) functions that allow you to write your own code for formatting the tooltip and index.

My code next constructs the query path for the REST web service using the config data and then gets the data using the jQuery $.getJSON() function.  The GeoJSON data is downloaded in batches.  You can often configure how many objects you wish to download in a GIS web service and you can configure how many batches you wish to retrieve in my example code.  Note that my examples use public web services maintained by the state of New York so I can’t guarantee they will continue working forever or have perfect data quality.

Instead of adding the objects one-by-one, this template uses a more recent release of Leaflet that allows you to add a set of JSON Features or a FeatureCollection to the Leaflet method L.geoJSON().   This method exposes the *onEachFeature* option that allows you to format or add functionality to each shape as it is created.  It also exposes the *filter* option that allows you to ignore some elements:
```javascript
          L.geoJSON(geoJSONData, {
            filter: geoJSONFilter,
            onEachFeature: onEachFeature
          }).addTo(lfmap);  
```

## Filtering Data

The “geoJSONFilter” function that is called by the *filter* option of the L.geoJSON()function can be used for ignoring shapes that have a duplicate Unique ID as in my example:
```javascript
  // If a Unique ID property (uidProperty) is specified in config, 
  // Do not display shapes with a duplicate Unique ID.
  function geoJSONFilter(feature, layer) {
    if (!config.uidProperty) return true;

    var uid = (feature.properties[config.uidProperty]) ?
      feature.properties[config.uidProperty] :
      (config.uidProperty + ' is blank');
    for (var i = 0; i < dataSet.length; i++) {
      if (dataSet[i][2] == uid) {
        countDuplicateShapes++;
        return false;
      }
    }
    return true;
  }
```

## Formatting Your Shapes

The onEachFeature() function can be used to set the shape’s color and bind a formatted popup to the shape.  In my example, I also add the shape to the array that is used to create the index of shapes.  This code can use a standard layout or it can use a custom layout you define yourself if you create a getSearchTableData() function in the config object:           
```javascript
  // Called as each shape (feature) is drawn on the map.
  function onEachFeature(feature, layer) {
    var objType = feature.geometry.type.replace('Multi', ''); // User doesn't care if "multi" or not.
    var shapeNbr = dataSet.length + 1;
    layer.options.color = feature.properties.Color ? feature.properties.Color :
      getColorRotation();

    var title = formatTitle(objType, feature.properties);
    var text = formatPopupText(title, feature.properties, shapeNbr - 1);
    layer.bindPopup(text);
    mapObjects.push(layer);

    if (config.getSearchTableData) {
      dataSet.push(config.getSearchTableData(shapeNbr, objType, title, feature));
    } else {
      dataSet.push([shapeNbr, objType, title]);
    }
  }
```
getSearchTableData() example from [Leaflet_part7_rest_GeoJSON_more.htm](Leaflet_part7_rest_GeoJSON_more.htm):
```javascript
var schools = createConfig('New York Schools', 'Part of the state only.',
  'https://gisservices.its.ny.gov/arcgis/rest/services/NYS_Schools/FeatureServer/1/query',
  'LABEL_NAME,SED_CODE,PHYSCITY,PHYSZIPCD5,RECORD_TYPE_DESC',
  'LABEL_NAME', '');
  
schools.getSearchTableData = function(shapeNbr, objType, title, feature) {
  return [shapeNbr, objType, title, feature.properties.PHYSCITY,
    feature.properties.PHYSZIPCD5, feature.properties.RECORD_TYPE_DESC
  ];
};
```

## Other Enhancements

Another enhancement I made is to show you how to change the map tiles that are the geography images shown behind the objects you draw.  There are many different sets of tiles available from OpenStreetMap and other sources.  You can define the list in the tileLayers object array.  The objects in this array include a “maxZoom” attribute that is needed since many tile layers do not have as much close up detail available as other maps.  I used jQuery to populate a *select* input using this array.  The *select* input calls the drawTileLayer() function when changed:
```javascript
  function drawTileLayer(newTileLayerIndex) {
    var tileLayerSettings = tileLayers[newTileLayerIndex];
    if (lfTileLayer === undefined) {
      lfTileLayer = L.tileLayer(tileLayerSettings.URL, {
        maxZoom: tileLayerSettings.maxZoom,
        attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
      }).addTo(lfmap);
    } else {
      lfTileLayer.setUrl(tileLayerSettings.URL);
      if (tileLayerSettings.maxZoom < lfmap.getZoom()) {  // if map is currently closer allowed with new tiles, zoom back
        lfmap.setZoom(tileLayerSettings.maxZoom);
      }
    }
    lfmap.setMaxZoom(tileLayerSettings.maxZoom);
    tileLayerIndex = newTileLayerIndex;
  }
```

In the [second example](https://mattgingery.github.io/LeafletExamples/Leaflet_part7_rest_GeoJSON_more.htm), I created an array of configurations so that you can see visualizations of several different REST services.

Feel free to use these examples as a template for your future work.  Happy coding!
