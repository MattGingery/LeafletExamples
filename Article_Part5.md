# Leaflet Maps – JavaScript Library – Part 5 – Well Known Text and GeoJSON
This builds on the simple template described in [the previous article](https://mattgingery.github.io/LeafletExamples/Article_Part4).

Part 5 – Well Known Text and GeoJSON Example links: [Preview](https://mattgingery.github.io/LeafletExamples/Leaflet_part5_wkt.htm) | [GitHub](https://github.com/MattGingery/LeafletExamples/blob/master/Leaflet_part5_wkt.htm) | [JSFiddle](https://jsfiddle.net/mgingery/m4we37od/)

Note: if you use the JSFiddle link, you will want to go to “Settings” and choose “Right results” or “Tabs” so that you can better see the results.

## Well Known Text

In all of the previous examples, we used arrays of lat/long values to define polygons, markers, and lines.  In many cases, such as if you store your shapes data in a database as geography or geometry data types, the easiest way to transfer the shape definitions to your web page is using [“Well Known Text” (WKT)](https://en.wikipedia.org/wiki/Well-known_text#Geometric_objects).  For example, a Marker with a lat/long of “32.794033, -117.250707” would have a WKT value of 'POINT( -117.250707 32.794033)'.  Note that WKT and GeoJSON have the longitude values first, followed by the latitude, whereas the Leaflet component accepts the latitude value first in its polygon and marker creation methods.

## GeoJSON

In addition, you may have existing web services that return shape values as [GeoJSON](http://geojson.org/).  An example of a GeoJSON string would be:  '{"coordinates": [-117.250707, 32.794033], "type": "Point"}'.  You could parse WKT or GeoJSON yourself, but I would suggest a much simpler option:

## The Wicket Javascript Library

[The Wicket Javascript Library](https://arthur-e.github.io/Wicket/) allows you to input a WKT or GeoJSON string and it will output a Leaflet object with just a few lines of code.  First, you add the reference to the Wicket library in the html head:
```javascript
  <script src="https://arthur-e.github.io/Wicket/wicket.js" type="text/javascript"></script>
  <script src="https://arthur-e.github.io/Wicket/wicket-leaflet.js" type="text/javascript"></script>  
```
Next, you instantiate the Wicket object and read some WKT or GeoJSON text into the object:
```javascript
var wkt = new Wkt.Wkt();
wkt.read('POINT( -117.250707 32.794033)');

```
Finally, you execute the toObject() method to create the Leaflet object that you can then add to the map.  Optionally, you can specify Leaflet object properties when you create the object such as “color”:
```javascript
var obj = wkt.toObject({color: 'red'});    
obj.addTo(lfmap).bindPopup(formatPopupHtml);
```

## Integrating Wicket into the Example
Using this library also makes your Javascript and middle tier code shorter and easier to maintain.  This is because you can output any combination of WKT & GeoJSON points, polygons, and lines from a single place instead of having to have different sets of code for returning and creating each type of object.  Wicket usage comes with a slight performance overhead over the standard method of creating Leaflet objects, but in my tests it is completely unnoticeable if you create fewer than 10,000 rectangles (or fewer than that if your shapes are more complex).  

In my new sample code, the function **drawLayers()** now only has to iterate through one array of objects instead of separate arrays of points, lines, and polygons.  I also added some code to the function **formatTitle()** so that the default name (or “title”) of each object will be formatted based on the “type” property of the Wicket variable object variable (“wkt”).  I created a global variable drawnObjTypes that will hold the counts of linestring, point, and polygon objects so the code can give each object a sequential name in the index.

## Next Steps
This example has a lot of functionality but still has a few problems.  You may notice that when you click on some of the lines in the index, it shows the pop-up in the middle of the bounds of the line but it does not always touch the line.  In [future examples](https://mattgingery.github.io/LeafletExamples/Article_Part6) I will show how to fix that issue and also show how load batches of drawn objects asynchronously so that the web page does not hang for a long time while it is drawing large numbers of objects (e.g. more than 10,000 rectangles).
