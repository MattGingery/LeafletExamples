# Leaflet Maps JavaScript Library - Examples
## Introduction
More and more often, websites are using maps to display location data to their users.  Most of the time, I see sites using Google Maps and occasionally you will see them use Mapquest or Bing Maps.  These are the sites people are most familiar with so web development teams often have to go through the process of purchasing an API from one of these companies, installing the API, learning the API, and making sure the license keys are provided to developers, testers, and administrators.  You have to make sure you re-purchase the license periodically and hope that they do not decide to increase the charge so much that it breaks your product’s budget.  These third-party components also make your application builds more complex, likely to have errors, and limit your flexibility.  Many developers have found out that a third-party component no longer works when you try to release new programming language versions or server infrastructure.  Don’t you wish you could avoid all of that?

You can with a very simple to use, open-source JavaScript library named Leaflet (https://leafletjs.com).  The Leaflet library can be used to display interactive maps from several sources but most commonly uses the OpenStreetMap map data (https://www.openstreetmap.org).  I have found the quality of these maps to be great in the places I have tested.  You can draw markers and shapes on the map easily, create pop-ups for each item you draw, and animate movements across the map.  

## My Examples

The documentation for Leaflet is pretty good and I found several examples online that show how to do one small part of the code for a page but no good tutorials that show a complete working solution.  I have written several articles to fill that need.  

For a **beginner's tutorial**, start with [my first article](https://mattgingery.github.io/LeafletExamples/Article_Part1).  

To jump straight to an **example** that has lots of functionality already built, see [Leaflet_part6_concavePolygons.htm](https://mattgingery.github.io/LeafletExamples/Leaflet_part6_concavePolygons.htm).

I also created some other articles each with example files that show step-by-step how you can start with a list of markers and/or polygons, draw each one, give each polygon specified color, display data when a marker or polygon is clicked, create a clickable index of the mapped objects, and more.  I have them available for download at https://github.com/MattGingery/LeafletExamples/ or for displaying at https://mattgingery.github.io/LeafletExamples/ and https://jsfiddle.net/user/mgingery/fiddles/ (the code on JSFiddle is slightly different since JSFiddle sometimes displays differently than a stand-alone HTM file).

I used ECMAScript 5 Javascript in these examples so that this code would work in all Browsers including I.E.    

For more information about me, visit [my LinkedIn page](https://www.linkedin.com/in/MattGingery).

## Tutorial Table of Contents:

Example Link | JSFiddle Link | Article Link
------------ | ------------- | -------------
[1.](https://mattgingery.github.io/LeafletExamples/Leaflet_part1_simple.htm) | [fiddle](https://jsfiddle.net/mgingery/zd7upbx2/) | [Part 1 – A Simple Template](https://mattgingery.github.io/LeafletExamples/Article_Part1) 
[2.](https://mattgingery.github.io/LeafletExamples/Leaflet_part2_autoCenterAndZoom.htm) | [fiddle](https://jsfiddle.net/mgingery/ght9n73c/) | [Part 2 – Auto Center and Zoom](https://mattgingery.github.io/LeafletExamples/Article_Part2)
[3.](https://mattgingery.github.io/LeafletExamples/Leaflet_part3_userInteraction.htm) | [fiddle](https://jsfiddle.net/mgingery/j1zme7qr/) | [Part 3 – User Interaction & Rotate Colors](https://mattgingery.github.io/LeafletExamples/Article_Part3)
[4.](https://mattgingery.github.io/LeafletExamples/Leaflet_part4_index.htm) | [fiddle](https://jsfiddle.net/mgingery/689pyjLm/) | [Part 4 – Map Index](https://mattgingery.github.io/LeafletExamples/Article_Part4)
[5.](https://mattgingery.github.io/LeafletExamples/Leaflet_part5_wkt.htm) | [fiddle](https://jsfiddle.net/mgingery/m4we37od/) | [Part 5 – Well Known Text and GeoJSON](https://mattgingery.github.io/LeafletExamples/Article_Part5)
[6.](https://mattgingery.github.io/LeafletExamples/Leaflet_part6_concavePolygons.htm) | [fiddle](https://jsfiddle.net/mgingery/6x3z9reL/) | [Part 6 – Concave Polygons](https://mattgingery.github.io/LeafletExamples/Article_Part6)

