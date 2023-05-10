<template>
  <div class="container">
    <div class="LCol">
      <button v-on:click="AOI(1.325184, 103.899868)" class="BtnShaper">
        Area Of Interest 1
      </button>
      <button v-on:click="AOI(1.355563, 103.831749)" class="BtnShaper">
        Area Of Interest 2
      </button>
      <button v-on:click="AOI(1.337135, 104.008439)" class="BtnShaper">
        Area Of Interest 3
      </button>
      <button v-on:click="AOI(1.256128, 103.817942)" class="BtnShaper">
        Area Of Interest 4
      </button>
      <button v-on:click="AOI(1.30387, 103.876689)" class="BtnShaper">
        Reset AOI
      </button>
    </div>
    <div class="MCol">
      <l-map
        ref="map"
        style="height: 95vh; width: 60vw"
        :zoom="zoom"
        :center="center"
        @update:zoom="zoomUpdated"
        @update:center="centerUpdated"
        @update:bounds="boundsUpdated"
      >
        <!-- <l-tile-layer :url="url">

        </l-tile-layer> -->
        <l-control-layers position="topright"></l-control-layers>
        <l-tile-layer
          v-for="tileProvider in tileProviders"
          :key="tileProvider.name"
          :name="tileProvider.name"
          :visible="tileProvider.visible"
          :url="tileProvider.url"
          :attribution="tileProvider.attribution"
          layer-type="base"
        />

        <l-draw-toolbar position="topright"></l-draw-toolbar>
        <l-circle :lat-lng="circle.center" :radius="circle.radius" />
        <l-rectangle :bounds="rectangle.bounds" :l-style="rectangle.style" />
        <l-polygon :lat-lngs="polygon.latlngs" :color="polygon.color" />

        <l-marker :lat-lng="markerLatLng">
          <l-popup>AOI</l-popup>
          <!-- <l-icon :icon-size="brew.iconSize" :icon-url="icon" /> -->
        </l-marker>

        <l-marker :lat-lng="[47.41322, -1.199482]">
          <l-icon icon-url="icon"> </l-icon>
        </l-marker>

        <l-control-scale
          position="bottomleft"
          :imperial="true"
          :metric="false"
        ></l-control-scale>
      </l-map>
      <div class="GeoTextContainer">
        <span class="GeoText"> zoom: {{ zoom }} </span>
        <span class="GeoText"> center: {{ center }} </span>
      </div>
    </div>
    <div class="RCol">
      <button v-on:click="Polygon()" class="BtnShaper">Show Polygon</button>
      <button v-on:click="Circle()" class="BtnShaper">Show Circle</button>
      <button v-on:click="Rectangle()" class="BtnShaper">Show Rectangle</button>
      <!-- <button class="BtnShaper">Show Terrain Mark</button> -->
    </div>
  </div>
</template>


// https://vdcrea.gitlab.io/vue-leaflet/#licon

<script>
import {
  LMap,
  LTileLayer,
  LMarker,
  LPopup,
  LPolygon,
  LRectangle,
  LCircle,
  LIcon,
  LControlLayers,
  LControlScale,
} from "vue2-leaflet";

import { latLng } from "leaflet";
import LDrawToolbar from "vue2-leaflet-draw-toolbar";
import logo from "../src/assets/logo.png";

export default {
  components: {
    LMap,
    LTileLayer,
    LDrawToolbar,
    LMarker,
    LPopup,
    LPolygon,
    LRectangle,
    LCircle,
    LIcon,
    LControlLayers,
    LControlScale,
  },
  data() {
    return {
      // url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      zoom: 15,
      markerLatLng: [1.30387, 103.876689],
      center: [1.30387, 103.876689],
      bounds: null,
      icon: logo,
      iconSize: [20, 20],
      tileProviders: [
        {
          name: "OpenStreetMap",
          visible: true,
          attribution:
            '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
          url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
        },
        {
          name: "OpenTopoMap",
          visible: false,
          url: "https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png",
          attribution:
            'Map data: &copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>, <a href="http://viewfinderpanoramas.org">SRTM</a> | Map style: &copy; <a href="https://opentopomap.org">OpenTopoMap</a> (<a href="https://creativecommons.org/licenses/by-sa/3.0/">CC-BY-SA</a>)',
        },
      ],
      circle: {
        center: latLng(47.41322, -1.0482),
        radius: 4500,
      },
      rectangle: {
        bounds: [
          [47.341456, -1.397133],
          [47.303901, -1.243813],
        ],
        style: { color: "red", weight: 5 },
      },
      polygon: {
        latlngs: [
          [47.2263299, -1.6222],
          [47.21024000000001, -1.6270065],
          [47.1969447, -1.6136169],
          [47.18527929999999, -1.6143036],
          [47.1794457, -1.6098404],
          [47.1775788, -1.5985107],
          [47.1676598, -1.5753365],
          [47.1593731, -1.5521622],
          [47.1593731, -1.5319061],
          [47.1722111, -1.5143967],
          [47.1960115, -1.4841843],
          [47.2095404, -1.4848709],
          [47.2291277, -1.4683914],
          [47.2533687, -1.5116501],
          [47.2577961, -1.5531921],
          [47.26828069, -1.5621185],
          [47.2657179, -1.589241],
          [47.2589612, -1.6204834],
          [47.237287, -1.6266632],
          [47.2263299, -1.6222],
        ],
        color: "#ff00ff",
      },
    };
  },
  methods: {
    zoomUpdated(zoom) {
      this.zoom = zoom;
    },
    centerUpdated(center) {
      this.center = center;
    },
    boundsUpdated(bounds) {
      this.bounds = bounds;
    },
    AOI(Lat, Long) {
      this.center = [Lat, Long];
      this.markerLatLng = [Lat, Long];
    },
    Polygon() {
      this.polygon = {
        latlngs: [
          [1.313121, 103.888389],
          [1.318114, 103.88658],
          [1.319554, 103.895012],
          [1.311038, 103.900745],
          [1.305218, 103.892283],
          [1.313121, 103.888389],
        ],
        color: "green",
      };
      this.center = [1.311038, 103.90074];
      this.markerLatLng = [1.311038, 103.90074];
    },
    Rectangle() {
      this.rectangle = {
        bounds: [
          [1.311408, 103.849619],
          [1.306414, 103.859553],
        ],
        style: { color: "red", weight: 3 },
      };
      this.center = [1.311408, 103.849619];
      this.markerLatLng = [1.311408, 103.849619];
    },
    Circle() {
      this.circle = {
        center: latLng(1.33021, 103.846738),
        style: { color: "orange", weight: 4 },
        radius: 350,
      };
      this.center = [1.33021, 103.846738];
      this.markerLatLng = [1.33021, 103.846738];
    },
  },
};
</script>

<style>
.container {
  display: flex;
  height: 100vh;
  width: 100vw;
}

.container div {
  display: inline-block;
}

.MCol {
  width: 60vw;
  height: 100vh;
}

.MCol div.GeoTextContainer {
  display: block;
}

.LCol,
.RCol {
  padding: 0 10px;
  width: 15vw;
  height: 100vh;
  background: #444;
}

div.GeoTextContainer {
  text-align: center;
}
.GeoText {
  font-size: 18px;
}
.BtnShaper {
  padding: 10px 0;
  width: 100%;
  font-size: 14px;
  margin: 10px 0;
}

/* https://unpkg.com/leaflet@1.6.0/dist/images/layers-2x.png */

.leaflet-retina .leaflet-control-layers-toggle {
  background-image: url("../src/assets/layers-2x.png") !important;
}
</style>