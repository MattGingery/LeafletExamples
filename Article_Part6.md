# Leaflet Maps – JavaScript Library – Part 6 – Concave Polygons
This builds on the simple template described in [the previous article](https://mattgingery.github.io/LeafletExamples/Article_Part5).

Part 6 – Concave Polygons Example links: [Preview](https://mattgingery.github.io/LeafletExamples/Leaflet_part6_concavePolygons.htm) | [GitHub](https://mattgingery.github.io/LeafletExamples/Leaflet_part6_concavePolygons.htm) | [JSFiddle](https://jsfiddle.net/mgingery/6x3z9reL/)

## Dealing with Concave Polygons

In the [previous examples](https://mattgingery.github.io/LeafletExamples/Leaflet_part5_wkt.htm), you may have noticed that when you click on some of the shapes listed in the index, it shows the pop-up close to the line/shape but it does not always touch the polyline or polygon.  That is because the center of the bounding box around the polyline or polygon will sometimes be inside the “dent” of a [concave]( http://www.differencebetween.info/difference-between-concave-and-convex-polygons) polygon or polyline.  Single lines, rectangles, and triangles do not have this problem, but irregular shapes such as an “L”-shaped polygon sometimes do.  

To help show and test this issue, I created [some example code](https://mattgingery.github.io/LeafletExamples/Leaflet_part6_centroidTest.htm) that shows a series of concave polygons or polylines [\*](#footnotes), a dotted bounding box around each one, and markers representing the various methods you can use to determine the visual center of the object.  In most cases, the bounding box center was quite far from the visual center of each shape.  Another method you can use to try to find the visual center of the object would be to find the [centroid](https://en.wikipedia.org/wiki/Centroid) of the polygon.  In more recent versions of Leaflet you can use the polyline.getCenter() method to get the centroid of an object.  You can also convert the lat/long values to an array and then use a **getCentroid()** function that you can copy from my example or find easily enough online.  Centroid usually looks a little bit closer to the visual center of the object but still often does not touch the polygon.

Another method you can use for determining the center of an object is an iterative grid algorithm.  I found one such example at https://github.com/mapbox/polylabel.  I modified it slightly for use in straight JavaScript and tested it out on the example as well.  The results were sometimes close to the centroid and usually a little better quality but still often did not touch concave polygons.  None of the standard algorithms seems to work well for concave polygons; is there another solution?

## Testing Whether a Point is within a Polygon

My suggested method is to calculate the bounding box center value since that is quick and simple and works for fine for most shapes and then test whether it exists inside the polygon.  Only if it does NOT exist inside the polygon, or if the shape is a multiple segment polyline, do we need to do anything else to determine the visual center.  

To do this determination, I found a good library called [Leaflet.PointInPolygon](https://github.com/hayeswise/Leaflet.PointInPolygon) by Brian Hayes.  Simply download the [wise-leaflet-pip.js ](https://github.com/hayeswise/Leaflet.PointInPolygon/blob/master/wise-leaflet-pip.js) file from his Github and make a link to it in your html page.  Then you can use the Polyline.contains() extension method from the downloaded “pip” file:
```javascript
var bounds = new L.featureGroup([obj]).getBounds();
var centerLatLng = bounds.getCenter();
if (!obj.contains(centerLatLng)) {
        // do additional work here…
      }
```
This method will come in handy for displaying the visual center for concave polygons.

## When getCenter() is Not Within the Polygon…

Since centroid and other methods do not work well for finding the visual center of concave polygons, we must explore other options when we find that the bounding box center is not within the polygon.  The easiest method would be to use one of the corners of the polygon as the “center”.  You can get the array of LatLng coordinates by simply using the Leaflet objects’ **getLatLngs()** method.  After experimenting with a few shapes using different methods, I found the best looking logic is to find the middle coordinates of each line in the polygon or polyline and use the one that is closest to the bounding box center.  First, I created a **getMiddleOfTwoLngLng()** function:
```javascript
function getMiddleOfTwoLngLng(latLng1, latLng2) {
  var lat = (latLng1.lat + latLng2.lat) / 2;
  var lng = (latLng1.lng + latLng2.lng) / 2;
  return new L.LatLng(lat, lng);
} 
```  
Second, create a function that iterates though each coordinate in the object, gets the middle point of the line associated with the coordinate and the next one, and then returns the middle point that has the shortest .distanceTo(theBoundingBoxCenter) value.  Note that the number of iterations will be 1 fewer for polylines since polygons assume that the first and last coordinates form a line that closes off the shape (unlike polylines).  For example, a polyline with a getLatLngs() array of 3 coordinates could have an “L” shape, but a polygon with the same 3 coordinates would be displayed as a triangle.
```javascript
function getClosestMiddleOfObjToLatLng(objLatLngs, latLng, isLine) {
  var middleLatLng, distance = Number.MAX_VALUE;
  var nbrLines = objLatLngs.length - (isLine ? 1 : 0); // get closing LatLng if polygon	
  for (var i = 0; i < nbrLines; i++) {
    var nextCoord = objLatLngs[(i == objLatLngs.length - 1) ? 0 : i + 1] ; 
    var thisMiddle = getMiddleOfTwoLngLng(objLatLngs[i], nextCoord ) ;
    var thisDistance = latLng.distanceTo(thisMiddle);
    if (thisDistance < distance) {
      middleLatLng = thisMiddle;
      distance = thisDistance;
    }
  }
  return middleLatLng;
}
```  
Next, we need a method to determine the type of an object (Marker, Polyline, or Polygon) that was drawn on a map.  Unfortunately, Leaflet does not seem to have a reliable method for determining object type after you have created a Leaflet object so we need to store this information somewhere when you add the object to the map.  To do this, I will just determine the type when I read the data, and store it as a new array element in the “dataSet” global variable.
```javascript
var objType = 'Point';  
if (wkt.type.indexOf('line') > -1) {
	objType = 'Line' ; 
}
else if (wkt.type.indexOf('polygon') > -1 ) {
	objType = 'Polygon' ; 
}
dataSet.push([title, index + 1, objType])
```
Finally, we add must the new logic to the existing **displayMapObject()** function that determines if the bounding box center is within the object and get the closest middle of the object’s line segments if it is not.  
```javascript
function displayMapObject(mapObjectsIndex) {
  zoomToMapObject(mapObjectsIndex);
  var obj = mapObjects[mapObjectsIndex];
  var group = new L.featureGroup([obj]);
  var centerLatLng  = group.getBounds().getCenter() ;
  var objType = dataSet[mapObjectsIndex][2] ; 
  
  // If bounding box center is not within the polygon, find the closest 
  // corners to that lat/long and place marker in the middle of it.
  if ( objType == 'Line' || ( objType == 'Polygon' && !obj.contains(centerLatLng) ) ) {
	var latLngs = obj.getLatLngs();
	centerLatLng = getClosestMiddleOfObjToLatLng(latLngs, centerLatLng, (objType == 'Line' ) );
  }  
  
  obj.openPopup(centerLatLng );
} 
```

## Next Steps

This example provides a pretty good starting point for creating simple interactive mapping applications.  If you can have your middle tier application provide GeoJSON or simple arrays of coordinates, you will probably want to change this template to load shapes using the [L.geoJSON object](https://leafletjs.com/reference-1.3.4.html#geojson) instead of the Wicket third party script since Wicket does not seem to work with the latest versions of Leaflet.

In the future, I may add an example of that or add other commonly used functionality.

## Footnotes
1. For creating these shapes, I used a [Leaflet draw example page](https://bl.ocks.org/danswick/d30c44b081be31aea483) created by Dan Swick.  There may be better sites for doing this; this was just the first I found that works. 

