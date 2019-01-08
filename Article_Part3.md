# Leaflet Maps – JavaScript Library – Part 3 – User Interaction & Rotate Colors 

This builds on the simple template described in [the previous article](https://mattgingery.github.io/LeafletExample/Article_Part2).

Part 3 – User Interaction & Rotate Colors Example links: [Preview](https://mattgingery.github.io/LeafletExamples/Leaflet_part3_userInteraction.htm) | [GitHub](https://github.com/MattGingery/LeafletExamples/blob/master/Leaflet_part3_userInteraction.htm) | [JSFiddle](https://jsfiddle.net/mgingery/j1zme7qr/)

## User Interaction
In the previous examples, all of the JavaScript code was run when the page was loaded, but you can also have Leaflet JavaScript functions run as the result of an action by the user.  In this case, I will add code that will allow the user to zoom in to a selected object using a hyperlink click and then zoom back out with a different click.
First, we must re-factor the code so that the Leaflet objects created can be accessed later.  You will see on the example code that I added a new link and changed some of the local variables to global variables included the Leaflet Map object itself (“lfmap”) and the array of shapes and markers drawn on the map (“mapObjects”).  The PopUp of each object now has a new link that, when clicked, will zoom in to the object selected.  The new link will re-run the code that auto-centers and sets the zoom level to show all of the objects.

One note about the JSFiddle page: make sure you set the Load Type to “No wrap – bottom of \<body\>” if you use global variables in your own tests.  You can access that setting using drop down at the top of the JavaScript editor panel.

## Improving the Display of the Shapes

When I first tested a Leaflet webpage I created some time ago, I noticed it was somewhat hard to identify the boundary and easily recognize the difference between adjacent shapes.  One way to improve the display of polygons on the map is to rotate the color of the shape as you draw each one.  You could specify the colors provided to the getPolygons() array from the middle tier code if you like or you could create a JavaScript function to do that like what I did in this example.  The method is pretty simple: create a function **getColorRotation()** that keeps track of the color that was previously used and returns the next color in a defined array of colors.  You will probably have more difficulty finding a good set of colors that do not look horrible together when drawn on the page but contrast with the each other and the colors used on OpenStreetMap (or whatever map tiles you wish to use).

## Next Steps
In the [next article](https://mattgingery.github.io/LeafletExamples/Article_Part4), I will show you how to make a searchable index of the places on your map.
