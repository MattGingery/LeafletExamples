# Leaflet Maps – JavaScript Library – Part 4 – Index
This builds on the simple template described in [the previous article](https://github.com/MattGingery/LeafletExamples/blob/master/Article_Part3.md).

Part 4 –Index Example links:  [GitHub](https://github.com/MattGingery/LeafletExamples/blob/master/Leaflet_part4_index.htm) | [JSFiddle](https://jsfiddle.net/mgingery/689pyjLm/)

Note: if you use the JSFiddle link, you will want to go to “Settings” and choose “Right results” or “Tabs” so that you can better see the results.
## Finding What You Need
Once you start drawing dozens or hundreds of objects on an interactive map, it may become difficult for the user to find the particular shape or marker she or he is interested in.  In this example, I will show you how to display an index of the objects that are drawn on the map.  We will use another JavaScript library that allows you to sort, filter, do paging, and then click on an item to zoom to it on the map.

## jQuery DataTables Plug-in

The library I use most often to display data on a web-page is the [DataTables]( https://datatables.net/) plug-in for the jQuery JavaScript library.  It is free, has many features, and it is easy to set up.  I will use this to display the list of markers and shapes displayed in the example.

The first step is to add the references to jQuery and DataTables to the html head:

```javascript
  <script src="https://code.jquery.com/jquery-3.3.1.js"></script>
  <link rel="stylesheet" href="https://cdn.datatables.net/1.10.19/css/jquery.dataTables.min.css"/>
  <script                 src="https://cdn.datatables.net/1.10.19/js/jquery.dataTables.min.js"></script>
```
The code for creating a fully-functioning DataTable is as easy as:
```javascript
$('#searchTable').DataTable({
    data: dataSet,
	columns: [{
        title: 'Column Name 1'
      },
      {
        title: 'Column Name 2'
      }
    ]
});
```
In this example, “searchTable” is the id of the empty \<table\> element in the html, and “dataSet” can be a [two-dimensional array, objects, or instances](https://datatables.net/manual/data#Data-source-types).  I will show more settings later in this article.
## New HTML and CSS
The next step is to create some new HTML elements for the index.  I found that my site’s users preferred to see the map covering most of the screen, so I created a toolbar div with a button that can be used to show or hide the list of objects drawn on the map.  I then created another div below it containing the empty table that will be filled with data by the jQuery DataTables code on load:
```javascript
<div id="toolbar">
  <input type="button" id="searchButton" accesskey="S" title="Click or use Alt-S to hide/show the search page" value="Show Search" />
</div>
<div id="content">
  <div id="searchDiv">
    <table id="searchTable" class="display" width="100%"></table>
  </div>
  <div id="map"></div>
</div>
```
Next I added some CSS to place the index and the map side-by-side and hide the index by default: 
```javascript
  <style>
	#content {
	  width: 100%;
	  overflow: hidden;
	}

	#searchDiv {
	  display: none;
	  float: left;
	}

	#map {
	  width: 100%;
	  height: 95%;
	  float: right;
	}
  </style>  
```
## New JavaScript Code
Next I altered the JavaScript code from Example 3 to push the title and array index of each shape into a new global array as each item is drawn on the map:
```javascript
var dataSet = [];
function drawLayers() {
  var markers = getMarkers();
  for (var index = 0; index < markers.length; index++) {
…
    dataSet.push([title, index + 1]);
  };
  var objectsIndex = mapObjects.length;
  var shapes = getPolygons();
  for (var index = 0; index < shapes.length; index++) {
…
    dataSet.push([title, objectsIndex + 1])
    objectsIndex++;
  };
```
Then you will want to set up the [jQuery “ready”](https://learn.jquery.com/using-jquery-core/document-ready/) statement and move the call of the drawMap() function inside of it:
```javascript
$(document).ready(function() {
  drawMap(39.82835, -98.5816684, 5); // The Geographic Center of the United States
…
});
```
After that, you need to call the DataTables object create statement:
```javascript
  var table = $('#searchTable').DataTable({
    data: dataSet,
    pageLength: 15,
    pagingType: 'simple',
    bLengthChange: false,
    columns: [{
        title: ''
      },
      {
        title: ''
      }
    ],
    columnDefs: [{
      targets: 1,
      visible: false
    }]
  });
```
This includes several special settings to keep the index nice and compact including not showing headers (title: '').  In this example, I kept the object’s name as the first column so that it will be sorted by default, and hid the array index column.  In your application, you may want to add more columns such as address or other information that will help the user sort or filter the results.

After that, I created code to zoom to the map object selected and show its Pop-Up when you click on a row in the index:
```javascript
  $('#searchTable tbody').on('click', 'tr', function() {
    var index = table.row(this).data()[1] - 1;
    displayMapObject(index);
  });
```
Last, I added some standard jQuery code to show or hide the search div.  I found it helpful to the user to have the page zoom back to show all map objects when it shows the search div but you can remove that code if you do not like it.

## Next Steps
In these articles, we used arrays of lat/long values to define polygons, markers, and lines but sometimes your database or other data store will contain lists of objects with geometries defined by [Well Known Text (WKT)](https://en.wikipedia.org/wiki/Well-known_text#Geometric_objects). 
In future articles, I hope to show you how to handle WKT and show you how to handle the problems of Pop-Ups not appearing on the object for irregularly-shaped polygons or polylines.
