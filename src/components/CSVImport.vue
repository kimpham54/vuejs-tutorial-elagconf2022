<template>
  <div class="CSV-import bg-slate-300 p-6 h-full">
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
  <div>
    <l-map style="height: 600px" :zoom="zoom" :center="center">
      <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
      <l-marker :lat-lng="markerLatLng"></l-marker>
    </l-map>
  </div>
</template>

<script>
import "leaflet/dist/leaflet.css";
import { LMap, LTileLayer, LMarker } from "@vue-leaflet/vue-leaflet";
import {
  VueCsvToggleHeaders,
  VueCsvMap,
  VueCsvInput,
  VueCsvErrors,
  VueCsvImport,
} from "vue-csv-import";
export default {
  name: "CSVImport",
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
};
</script>
