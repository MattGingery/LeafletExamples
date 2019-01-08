# Leaflet Maps – JavaScript Library - Part 1 – A Simple Template 
## Minimum Requirements
All you need to create a working webpage is about 20 lines of html and JavaScript code:
```javascript
<html>
<head>
  <title>Leaflet Part1 - Minimum Required</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@0.7.3/dist/leaflet.css"/>
                  <script src="https://unpkg.com/leaflet@0.7.3/dist/leaflet.js"></script>
  <style>
    #map{ height: 100% }
  </style>  
</head>
<body>
<div id="map"></div>
<script>
var lfmap = L.map('map').setView([39.82835 , -98.5816684], 5);	

L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
	attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
}).addTo(lfmap);	
</script>
</body>
</html>  
```
1.	Set up links to the CSS and JS files that are on the Leaflet or Cloudflare website or you can download and host on your own website if you prefer.
2.	Create a \<div> where the map will be placed and set its height (height is required and width is not for some reason).
3.	Create a \<script> section AFTER the \<div>.  It will error if you create the script before the \<div>.
4.	Create a JavaScript variable that references the id of the div (‘map’), sets the center coordinates of the map (I used the geographic center of the continental U.S. in my example:  “[39.82835 , -98.5816684]”), and the zoom level (1 is the furthest and 18 is usually the closest you can get). 
5.	Create an object (“tileLayer”) using the above code and the call the “addTo()” method on it to place the map image tiles on the map object you created in the previous step.  You could specify other map sources here as well.

That is all it takes to display an interactive map on your own webpage.

## Drawing Shapes and Markers

Part 1 – Simple Example links:  [Preview](https://mattgingery.github.io/LeafletExamples/Leaflet_part1_simple.htm) | [GitHub](https://github.com/MattGingery/LeafletExamples/blob/master/Leaflet_part1_simple.htm) | [JSFiddle](https://jsfiddle.net/mgingery/zd7upbx2/) 

In the “Part 1 – Simple” example the getPolygons() and getMarkers() JavaScript functions are where you return the map objects you wish to draw.  Usually these would get their data from a web service or from the middle tier of you web application.  In this example, though, I put some sample objects in to show you the format of data they should return.  

**getMarkers()** should return an array of markers that are composed of 2 or more elements: Latitude, Longitude, Title, and data elements to display in the Popup when a marker is clicked.  In my example, I showed that you can store HTML here to display in the Popup, but it would be better practice to store the different data elements as multiple elements and then pass them all to the formatPopupText() function that will then transform it into the desired HTML you want displayed.

**getPolygons()** should also return an array of data composed of the polygon definition optionally followed by the title, Popup display data, and color of the polygon.  The polygon definition in Leaflet is itself an array or 3 or more arrays of a latitude value and a longitude value.  For example:

    [ [32.800807, -117.226163], [32.801618, -117.222419], [32.798083, -117.221303], [32.797325, -117.225219] ]

**drawMap()** must be called with the latitude and the longitude of the center of map to display as well as the zoom level.  It then calls drawLayers() which iterates the getMarkers() and getPolygons() returned arrays, sets defaults, then draws each object using drawMarker() or drawPolygon(), and assigns Popups for when you click on the drawn object.
## Next Steps
In the [next article](Article_Part2), I will show you how you can have the page automatically center and zoom to display the objects you draw on the map so that you will not have to manually set those values.
