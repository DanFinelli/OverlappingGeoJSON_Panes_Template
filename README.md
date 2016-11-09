# Layering overlapping polygon geoJSONs in Leaflet.js using panes and zindex. [HTML, JS]

You can see in the image below that there are 3 rectangle polygons. In order to allow hover or click actions on the smallest polygon it must not be overlapped by the larger ones.  We assigned different layer panes for each polygon and ordered them logically to solve the problem of large polygons overlapping small ones.

![Alt text](/TemplatePNGreadme.png "Overlapping GeoJSON Polygons")

## [Step 1] Panes = map layers that have an ordering sequence that can be defined.

In the script below the map variable:

> map.createPane('pane250'); 

Creates the pane, which was named ‘pane250’

> map.getPane('pane250').style.zIndex = 250;

Sets ‘pane250’ equal to a zindex value of ‘250’ 

> map.getPane('pane250').style.pointerEvents = 'none';


‘zindex’ is the sequential layering indicator tool. The higher the zindex value, the closer the layer will be to the webpage viewer/user. In example image, the red polygon geoJSON should have the highest zindex. Therefore, it will be closer to the user and they will be able to click it. The lower the zindex, the further away the layer is from the user. Ideally, the large green rectangle should be behind the other two so it does not interfere with the click targeting.

## Visualize the layering scheme: 

zindex ___________________1:	Furthest from user

zindex ___________________50:	On top of 1, but under 100

zindex ___________________100:	Closest to user

## Leaflet.js currently has panes set accordingly:

.leaflet-map-pane canvas { z-index: 100; }    

.leaflet-tile-pane    { z-index: 200; }     

.leaflet-map-pane svg    { z-index: 200; }	

.leaflet-pane         { z-index: 400; }

.leaflet-overlay-pane { z-index: 400; }

.leaflet-shadow-pane  { z-index: 500; }

.leaflet-marker-pane  { z-index: 600; }

.leaflet-tooltip-pane   { z-index: 650; }

.leaflet-popup-pane   { z-index: 700; }



The 3 overlapping polygons should be above the tile pane (the map tiles) which is set at 200. If the rectangle polygons are under 200, the user won’t be able to see them because the map tiles would be covering it. I also wanted to keep the panes below 400 so that other features of the maps, like popup windows, would still show above the overlapping polygons. Panes are relative, so as long as they are relative to a preferred layer ordering, the specific range of zindex number is not as important.


## [Step 2] Create a pane for however many panes needed:

Second Pane:

> map.createPane('pane251');

> map.getPane('pane251').style.zIndex = 251;

> map.getPane('pane251').style.pointerEvents = 'none';

Third Pane:

> map.createPane('pane252');

> map.getPane('pane252').style.zIndex = 252;

> map.getPane('pane252').style.pointerEvents = 'none';

## [Step 3] Load the geoJSON polygons and set the pane:


> var LARGEGEOJ;
> $.getJSON("https://raw.githubusercontent.com/ect123/Des-Barres-findingaid/master/maps/2-2a-27a.geojson",function(data){ 
> LARGEGEOJ = L.geoJson(data, {onEachFeature: onEachFeaturestylelargegeoj, pane: 'pane250'}).addTo(map);
>  });

Create a variable for the large polygon (LARGEGEOJ) and add the large geoJson to your map.  Set the map pane.  The largest polygon should be furthest away and in this case attached to the ‘pane250’.

## [Step 4] Add the remaining polygons and repeat step 3 with the appropriate pane:

> var MEDIUMGEOJ;
> $.getJSON("https://raw.githubusercontent.com/ect123/Des-Barres-findingaid/master/maps/2-2a-31.geojson",function(data){
> MEDIUMGEOJ = L.geoJson(data, {onEachFeature: onEachFeaturestylemediumgeoj, pane: 'pane251'}).addTo(map);
> });

> var SMALLGEOJ;
> $.getJSON("https://raw.githubusercontent.com/ect123/Des-Barres-findingaid/master/maps/2-2a-34.geojson",function(data){
> SMALLGEOJ = L.geoJson(data, {onEachFeature: onEachFeaturestylesmallgeoj, pane: 'pane252'}).addTo(map);
>  });


## Now all the polygons are loaded and set according to the appropriate zindex/pane that allows the user to click the smallest geoJSON regardless if it is being overlapped by a larger one.





