---
title: Places I've Been
---

<script src="http://cdnjs.cloudflare.com/ajax/libs/coffee-script/1.7.1/coffee-script.min.js"></script>
<script src="http://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>
<script src="http://www.openlayers.org/api/OpenLayers.js"></script>

<script id="locations" type="text/xml">
  <locations>
    <location longitude="-80.6695" latitude="41.0821" name="Youngstown" />
    <location longitude="-79.9963" latitude="40.4288" name="Pittsburgh" />
    <location longitude="-81.6940" latitude="41.4944" name="Cleveland" />
    <location longitude="-82.8191" latitude="41.6437" name="Put-In Bay" />
    <location longitude="-83.0051" latitude="39.9650" name="Columbus" />
    <location longitude="-73.9958" latitude="40.7189" name="New York City" />
    <location longitude="-71.0795" latitude="42.3526" name="Boston" />
    <location longitude="-77.0336" latitude="38.8917" name="Washington D.C." />
    <location longitude="-78.6909" latitude="35.7923" name="Raleigh" />
    <location longitude="-122.4660" latitude="37.7504" name="San Francisco" />
    <location longitude="-87.6833" latitude="41.8373" name="Chicago" />
    <location longitude="10.4102" latitude="53.2489" name="Lueneburg" />
    <location longitude="9.9973" latitude="53.5498" name="Hamburg" />
    <location longitude="13.3985" latitude="52.5182" name="Berlin" />
    <location longitude="8.6831" latitude="50.1102" name="Frankfurt am Main" />
    <location longitude="12.5512" latitude="55.6918" name="Koebenhavn" />
    <location longitude="13.2018" latitude="55.7090" name="Lund" />
    <location longitude="11.9514" latitude="57.7140" name="Goeteborg" />
    <location longitude="18.0665" latitude="59.3160" name="Stockholm" />
    <location longitude="6.9695" latitude="50.9410" name="Koeln" />
    <location longitude="8.8007" latitude="53.0750" name="Bremen" />
    <location longitude="10.4360" latitude="51.9153" name="Goslar" />
    <location longitude="9.7408" latitude="52.3695" name="Hannover" />
    <location longitude="10.6853" latitude="53.8645" name="Luebeck" />
    <location longitude="10.0761" latitude="52.6196" name="Celle" />
    <location longitude="9.4755" latitude="53.5933" name="Stade" />
    <location longitude="10.4889" latitude="53.1398" name="Bienenbuettel" />
    <location longitude="10.5594" latitude="52.9652" name="Uelzen" />
    <location longitude="10.5125" latitude="52.2650" name="Braunschweig" />
    <home longitude="13.7406" latitude="51.0509" name="Dresden" />
    <location longitude="12.38154" latitude="51.33865" name="Leipzig" />
    <location longitude="9.1952" latitude="48.7941" name="Stuttgart" />
  </locations>
</script>

<script type="text/coffeescript">
  $(document).ready ->
    map = new OpenLayers.Map("mapdiv")
    map.addLayer(new OpenLayers.Layer.OSM())

    zoom = 3

    xml = $($.parseXML($('#locations').text()))
    home = xml.find('home')
    locations = xml.find('location')

    createLonLat = (xml_node) ->
      longitude = parseFloat($(xml_node).attr('longitude'))
      latitude = parseFloat($(xml_node).attr('latitude'))

      lon_lat = new OpenLayers.LonLat(longitude, latitude)
        .transform(#Transform from WGS 1984.
                   new OpenLayers.Projection("EPSG:4326"),
                   #To Spherical Mercator Projection.
                   map.getProjectionObject())

    markers = new OpenLayers.Layer.Markers("Places to Which I've Been")
    map.addLayer(markers)

    home_lon_lat = createLonLat(home)
    markers.addMarker(new OpenLayers.Marker(home_lon_lat))
    map.setCenter(home_lon_lat, zoom)

    for location in locations
      lon_lat = createLonLat($(location))
      markers.addMarker(new OpenLayers.Marker(lon_lat))
</script>

<p>
  Below is a map showing all cities which I've visited which I can recall.
</p>
<div id="mapdiv" style="width: 100%; height: 600px; background-color: black;"></div>
