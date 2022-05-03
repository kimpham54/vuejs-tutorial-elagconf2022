<template>
  <div>
    <div class="CSV-import bg-slate-300 p-6">
      <vue-csv-import
        v-model="csv"
        :fields="{
          name: { required: true, label: 'Name' },
          location: { required: true, label: 'Location' },
        }"
      >
        <vue-csv-input></vue-csv-input>
        <vue-csv-errors></vue-csv-errors>
        <br />
        <vue-csv-toggle-headers />
        <vue-csv-map style="display: none" />
      </vue-csv-import>
      <br /><br />
      <table
        class="w-full text-sm text-left text-gray-500 dark:text-gray-400"
        v-if="csv"
      >
        <thead
          class="text-xs text-gray-700 uppercase bg-slate-50 dark:bg-gray-700 dark:text-gray-400"
        >
          <tr>
            <th>Name</th>
            <th>Location</th>
          </tr>
        </thead>
        <tbody class="bg-white border-b">
          <tr v-for="row in csv" :key="row">
            <td>{{ row.name }}</td>
            <td>{{ row.location }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    {{ markers }}
    <div>
      <l-map
        id="map-container"
        ref="myMap"
        :zoom="zoom"
        :center="center"
        @ready="doSomethingOnReady()"
      >
        <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
        <l-marker v-for="marker in markers" :lat-lng="marker" :key="marker">
        </l-marker>
      </l-map>
    </div>
  </div>
</template>

<script>
import "leaflet/dist/leaflet.css";
import { LMap, LTileLayer, LMarker } from "@vue-leaflet/vue-leaflet";
import axios from "axios";
import {
  VueCsvToggleHeaders,
  VueCsvMap,
  VueCsvInput,
  VueCsvErrors,
  VueCsvImport,
} from "vue-csv-import";
export default {
  name: "CSVPlaceFinder",
  components: {
    //HelloWorld,
    VueCsvToggleHeaders,
    VueCsvMap,
    VueCsvInput,
    VueCsvErrors,
    VueCsvImport,
    LMap,
    LTileLayer,
    LMarker,
  },
  data() {
    return {
      csv: null,
      markers: [],
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      zoom: 15,
      center: [51.505, -0.159],
      markerLatLng: [51.504, -0.159],
    };
  },
  async beforeMount() {
    // HERE is where to load Leaflet components!
    this.mapIsReady = true;
  },
  methods: {
    doSomethingOnReady() {
      this.map = this.$refs.myMap.leafletObject;
      //this.map.fitBounds(this.markers);
    },
  },
  watch: {
    markers(value) {
      console.log(value.length);
    },
    async csv(value) {
      console.log(value);
      this.markers = [];
      await value.forEach((value) => {
        console.log(value.location);
        axios
          .get(
            `http://api.geonames.org/searchJSON?name=${value.location}&maxRows=1&fuzzy=0.7&username=pelagios`
          )
          .then((response) => {
            const geonamesData = response.data.geonames[0];
            const placeName = geonamesData["toponymName"];
            const latitude = geonamesData["lat"];
            const longitude = geonamesData["lng"];
            console.log(`${placeName}, ${latitude}, ${longitude}`);
            if (!isNaN(parseFloat(latitude))) {
              this.markers.push([latitude, longitude]);
            }
            if (this.markers.length == this.csv.length) {
              console.log(this.markers);
              this.map.flyToBounds(this.markers, { padding: [100, 100] });
            }
          })
          .catch((e) => {
            console.log(e);
          });
      });
    },
  },
};
</script>
<style scoped>
#map-container {
  height: 600px !important;
}
</style>
