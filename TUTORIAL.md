## Tutorial Codealong

show final app product

APP GOALS
1. import a CSV
2. parse CSV
3. get coordinates from CSV
4. place coordinates on a map

go through installation 

```
vue create elag_vue_workshop
```

go through options create project
go through files


1. Remove template code - remove HelloWorld component from and template, script tag

remove in HelloWorld.vue
stuff in template

remove in App.vue
logo

rename your component from HelloWorld.vue to CSVPlaceFinder.vue (case sensitive, can't be all lowercase)

replace all declarations of HelloWorld with CSVPlaceFinder in CSVPlaceFinder.vue and App.vue


CSV IMPORTER - use https://www.npmjs.com/package/vue-csv-import

`yarn add vue-csv-import`


import libraries in script

```javascript
import {
  VueCsvToggleHeaders,
  VueCsvMap,
  VueCsvInput,
  VueCsvErrors,
  VueCsvImport,
} from "vue-csv-import";
```

declare use of the component in script export default

```javascript
  components: {
    VueCsvToggleHeaders,
    VueCsvMap,
    VueCsvInput,
    VueCsvErrors,
    VueCsvImport,
  },
```


use the component design the template in script export default
```html
    <vue-csv-import
      v-model="csv"
      :fields="{
        name: { required: true, label: 'Name' },
        location: { required: true, label: 'Location' },
      }"
    >
      <vue-csv-toggle-headers></vue-csv-toggle-headers>
      <vue-csv-errors></vue-csv-errors>
      <vue-csv-input></vue-csv-input>
      <vue-csv-map></vue-csv-map>
    </vue-csv-import>

```

pass data to a value

```javascript
  data() {
    return {
      csv: null,
    };
  },
```

display that value from v-model variable declaration (input binding) https://v2.vuejs.org/v2/guide/forms.html

```
    {{ csv }}
```

TURNS CSV INTO JSON ARRAY 

MAP TIME using https://github.com/vue-leaflet/vue-leaflet

```
yarn add @vue-leaflet/vue-leaflet
npm install leaflet
```

which modules from vue-leaflet do we need? at minimum? we need a map, tile layer, and markers

```javascript
import { LMap, LTileLayer, LMarker } from "@vue-leaflet/vue-leaflet";
import "leaflet/dist/leaflet.css";
```

```javascript
  components: {
    VueCsvToggleHeaders,
    VueCsvMap,
    VueCsvInput,
    VueCsvErrors,
    VueCsvImport,
    LMap,
    LTileLayer,
    LMarker,
  },
```


```html
    <l-map ref="myMap" style="height: 500px">
      <l-marker></l-marker>
      <l-tile-layer
        url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
      ></l-tile-layer
    ></l-map>
```

then add

```
:zoom="2"
```

we've set up a csv importer, we've set up a map. but they're not connected to each other right now. how would a connection look like?

csv importer
1. import a csv
1. csv gives you a name and a location

connection
1. from the location, get and store coordinates

map
1. put coordinates on the map as markers

to build the connection, we need a way to create coordinates. we use a geocoder called geonames. we also need a new library called axios, a popular http library that lets you work with apis

RETRIEVE GEONAMES API

- http://www.geonames.org/export/web-services.html
- http://www.geonames.org/export/geonames-search.html
- JSON search
- http://api.geonames.org/searchJSON?q=london&maxRows=10&username=demo
- http://api.geonames.org/searchJSON?q=london&maxRows=10&username=pelagios
- http://api.geonames.org/searchJSON?q=london&maxRows=1&username=pelagios



`yarn add axios`

`import axios from "axios";`


GET COORDINATES INTO JSON ARRAY
