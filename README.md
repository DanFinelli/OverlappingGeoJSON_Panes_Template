# Layering overlapping polygon geoJSON files using panes and zindex. [HTML]

In the image below, it can be see that there are 3 rectangle polygons (green, blue, and red). In the HTML code, one must indicate the layer order of these polygons so users are able to click the smaller red rectangle even though there are two encompassing polygons ‘overlapping’ it.

![Alt text](/TemplatePNGreadme.png "Overlapping GeoJSON Polygons")

## [Step 1] Panes create layers that have and ordering sequence which can be manufactured.

In the script and below the ‘var = map’ insert the following 3 lines of code:

> map.createPane('pane250'); 

This line creates the pane, which was named ‘pane250’

> map.getPane('pane250').style.zIndex = 250;

This line sets ‘pane250’ equal to a zindex value of ‘250’ 

> map.getPane('pane250').style.pointerEvents = 'none';


‘zindex’ is the sequential layering indicator tool. The higher the zindex value, the closer the layer will be to the webpage viewer/user. In example image, the red polygon geoJSON should have the highest zindex. Therefore, it will be closer to the user and therefore they will be able to click it. The lower the zindex, the further away the layer is from the user. Therefore, we would want the large green rectangle to be behind the other two so it does not interfere with the click targeting.

### This chart will help visualize the layering scheme: 

zindex ___________________1:	Furthest from user

zindex ___________________50:	On top of 1, but under 100

zindex ___________________100:	Closest to user

#### The current code from the CSS file of leaflet base tile map has panes set accordingly:

.leaflet-pane         { z-index: 400; }

.leaflet-tile-pane    { z-index: 200; }

.leaflet-overlay-pane { z-index: 400; }

.leaflet-shadow-pane  { z-index: 500; }

.leaflet-marker-pane  { z-index: 600; }

.leaflet-tooltip-pane   { z-index: 650; }

.leaflet-popup-pane   { z-index: 700; }

.leaflet-map-pane canvas { z-index: 100; }

.leaflet-map-pane svg    { z-index: 200; }	

These are preset in the 'CSS link href' located toward the top of the html document and do not need to be entered. This is only to visualize why first pane is set to a zindex of 250. The 3 overlapping polygons should be above the tile pane (the map) which is set at 200. If I set the rectangle polygons under 200, the user wouldn’t be able to see them because the map would be covering it. I also wanted to keep the panes below 400 to that other feature of the maps like popup windows would still show above the overlapping polygons.

If you have a different base file not from leaflet. Understand that the panes should be above the tile map. Panes are relative. So as long as they are relative to a preferred layer ordering, the specific range of zindex number is not as important.

Web pages automatically load HTML script from top to bottom. In theory you could just load the large polygon first and the small polygon last. Therefore, small polygon would load onto the previously loaded larger polygons. This may work, but it is not reliable due to inconsistent loading and refreshing errors. 
Similarly, using the BringToFront and BringToBack methods can work for a small number of overlapping layers. This method will incur inconsistent loading and refreshing errors if there are too many overlapping layers.

##### [Step 2] We must repeat step 1 for however many panes needed. In the example image there are 3 overlapping polygons. Therefore, 3 panes will be created for each. Enter the follow code after the 250 pane:

Second Pane:

> map.createPane('pane251');

> map.getPane('pane251').style.zIndex = 251;

> map.getPane('pane251').style.pointerEvents = 'none';

Third Pane:

> map.createPane('pane252');

> map.getPane('pane252').style.zIndex = 252;

> map.getPane('pane252').style.pointerEvents = 'none';

##### [Step 3] Next, load the geoJSON polygons and then dictate what pane they belong to. This should be done in the 'script' somewhere under the previous code:


> var LARGEGEOJ;

> $.getJSON("https://raw.githubusercontent.com/ect123/Des-Barres-findingaid/master/maps/2-2a-27a.geojson",function(data){
    
> LARGEGEOJ = L.geoJson(data, {onEachFeature: onEachFeaturestylelargegeoj, pane: 'pane250'}).addTo(map);

>  });

First a variable is created whichisnamed LARGEGEOJ. Then using AJAX the geoJSON is loaded. Next, set the variable equal to a ‘L.geoJson’ which styles the polygon using onEachFeature. The onEachFeature gives the style to the polygon, for example, the red color and opacity of the smallest geoJSON. This can be removed.

Finally, ‘pane: ‘pane250’’ tells the geoJSON is told which pane to live on using the panes given name. The largest polygon should be furthest away and was attached to the ‘pane250’.

##### [Step 4] Add the remaining polygons and repeat step 3 with the appropriate pane:

> var MEDIUMGEOJ;

> $.getJSON("https://raw.githubusercontent.com/ect123/Des-Barres-findingaid/master/maps/2-2a-31.geojson",function(data){
    
> MEDIUMGEOJ = L.geoJson(data, {onEachFeature: onEachFeaturestylemediumgeoj, pane: 'pane251'}).addTo(map);

 > });

> var SMALLGEOJ;

> $.getJSON("https://raw.githubusercontent.com/ect123/Des-Barres-findingaid/master/maps/2-2a-34.geojson",function(data){
    
> SMALLGEOJ = L.geoJson(data, {onEachFeature: onEachFeaturestylesmallgeoj, pane: 'pane252'}).addTo(map);

>  });


##### Now all the polygons are loaded and set according to the appropriate zindex/pane that allows the user to click the smallest geoJSON regardless if it is being overlapped by a larger one.


The Norman B. Leventhal Map Center used this method to create a finding aide index for its collection of Des Barres Atlantic Neptune historic nautical charts and views.
Browse the collection here: http://maps.bpl.org/explore/publisher/des-barres-joseph-fw-6 




