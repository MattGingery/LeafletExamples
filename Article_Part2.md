# Leaflet Maps – JavaScript Library - Part 2 – Auto Center and Zoom

This builds on the simple template described in [the previous article](https://github.com/MattGingery/LeafletExamples/blob/master/Article_Part1.md).

Part 2 – Auto Center and Zoom Example links:  [GitHub](https://github.com/MattGingery/LeafletExamples/blob/master/Leaflet_part2_autoCenterAndZoom.htm) | [JSFiddle](https://jsfiddle.net/mgingery/ght9n73c/)

## Automate the Bounds of the Map that is Displayed

When a user wants to see a map with some small mapping objects, they will be very annoyed if you show them a map of the world where the desired map elements are not even visible.  They will also be understandably annoyed if all of the objects are at the edge of the page.

In the code for the previous article, center coordinates and zoom level would have had to be determined ahead of time and entered as parameters of the drawMap() function to make sure all the objects are visible.  In this example, I instead set default map to view the continental United States (you should, of course, change this to show the country or region of the world where most of your website’s users are located).  I then added code to determine the center of the drawn objects and then zoom in to view all of them.

You will notice in the code that the drawMarker() and drawPolygon() functions return the marker or polygon object that is created.  As they are created, the code pushes them into an array variable called mapObjects[].  Once those objects are created, the code to adjust the map to nicely display the objects is simple:

```javascript
  var group = new L.featureGroup(mapObjects);
  lfmap.fitBounds(group.getBounds());
```

If you like, you can create these “featureGroup” objects as global variables containing a subset of the objects you display and then have an event such as a button click on the form move and zoom to show just the selected objects.

## Next Steps

In the [next article](https://github.com/MattGingery/LeafletExamples/blob/master/Article_Part3.md), I will add some user interaction code and to automatically adjust the polygon’s colors to give the users a better visual experience.

