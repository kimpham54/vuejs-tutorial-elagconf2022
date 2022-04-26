<template>
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
  <div>MAP</div>
</template>

<script>
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
  },
  data() {
    return {
      csv: null,
    };
  },
};
</script>
