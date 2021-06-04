<template>
  <div id="mapContainer" class="basemap"></div>
</template>

<script>
import mapboxgl from "mapbox-gl";
import "mapbox-gl/dist/mapbox-gl.css";
import MapboxDirections from "@mapbox/mapbox-gl-directions/dist/mapbox-gl-directions";
import "@mapbox/mapbox-gl-directions/dist/mapbox-gl-directions.css";

export default {
  name: "BaseMap",
  data() {
    return {
      accessToken:
        "pk.eyJ1IjoibWFwcy1tdWtleXNoIiwiYSI6ImNrcGR1Z280czAyMHIyb28yOXljY2FxNXQifQ.Vq8YcCWssYkRqICzlfT3SQ",
    };
  },
  mounted() {
    mapboxgl.accessToken = this.accessToken;
    const map = new mapboxgl.Map({
      container: "mapContainer",
      style: "mapbox://styles/mapbox/streets-v11", // style URL
      center: [78.9629, 20.5937], // starting position [lng, lat]
      zoom: 5, // starting zoom
    });
    const directions = new MapboxDirections({
      accessToken: mapboxgl.accessToken,
      unit: "metric",
    });
    map.addControl(directions, "top-left");
    directions.on("route", (e) => {
      console.log(e);
      let route = e.route;
      route.map((item) => {
        let distance = item.distance / 1000;
        let distanceRound = Math.round(distance);
        console.log(distanceRound + "KM");
      });
    });
  },
};
</script>

<style>
body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
    "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.basemap {
  width: 100%;
  height: 100%;
}
body .basemap {
  height: 100%;
  position: absolute;
  top: 0;
  bottom: 0;
  width: 100%;
}
.mapboxgl-ctrl-geocoder ul,
.mapbox-directions-step {
  text-align: left;
}
</style>
