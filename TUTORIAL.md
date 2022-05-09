## Tutorial Codealong
https://github.com/kimpham54/vuejs-tutorial-elagconf2022/blob/main/src/components/CSVPlaceFinder.vue

PASCAL START HERE

show diagram

show final app product

https://vuejs.org/guide/introduction.html

some terms - components

methods - functions

hooks - actions that happen when something happens


APP GOALS
1. import a CSV
2. parse CSV
3. get coordinates from CSV
4. place coordinates on a map


```
vue create elag_vue_workshop
```

go through installation 

test installation components node -version etc

do what it says with commands cd elag_workshop_etc

yarn serve

development server

put in localhost address

shutdown app ctrl z

open folder in app visual code studio

go through options create project

### go through files:

- package.json (scripts, dependencies, devdependencies, configuration options)
- src folder
- main.js - where we mount our vue application into our index, importing App from App.vue file
- App.js - our first main component, uses vue syntax. template tag is html, script tag is javascript, defines first main component App, additional components. parent component App uses child components HelloWorld, style options in css
- HelloWorld.vue - more elaborate template, script component defined name HelloWorld, props displays any kind of data that is defined in component into html. with props you can pass data from parent component into child component. *** state changes should flow downwards

1. Remove template code - remove HelloWorld component from and template, script tag

remove in HelloWorld.vue
stuff in template

remove in App.vue
logo

rename your component from HelloWorld.vue to CsvViewer.vue (case sensitive, can't be all lowercase)

replace all declarations of HelloWorld with CsvViewer in CsvViewer.vue and App.vue


```
<template>
  <div>

  </div>
</template>

<script>
export default {
  name: "CsvViewer",
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>


```

```
<template>
  <CsvViewer />
</template>

<script>
import CsvViewer from "./components/CsvViewer.vue";

export default {
  name: "App",
  components: {
    CsvViewer,
  },
};
</script>

<style>
</style>

```
- remove everything, have nothing http://localhost:8080/

KIM TAKE OVER HERE

- google csv upload vue
- npm global library register modules. we are using vue3 check versions compatibility and changes. components compatible with older versions
- have to shut down app, have to restart app and install modules

CSV IMPORTER - use https://www.npmjs.com/package/vue-csv-import

`yarn add vue-csv-import`

- yarn serve again, inspect and see that it is your page even if blank. console. integrate csv compoennt into csv viewer 


import components individually into script into CSvViewer.vue

```javascript
import {
  VueCsvToggleHeaders,
  VueCsvMap,
  VueCsvInput,
  VueCsvErrors,
  VueCsvImport,
} from "vue-csv-import";
```

- save but strict, defining all these components but never use them
- makes sure code is clean, have to make sure what you define is actually being used


- next use the component design the template in script export default
- v model explain later
- we know our what we need from our csv, we add name and location. take a look at csv file
- add extra components

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


- next declare use of the component in script export default for linter to be happy, needs to be registered in order for app to understand that they exist in html template, reproduce list from import into export recognized in the template


```javascript
  components: {
    VueCsvToggleHeaders,
    VueCsvMap,
    VueCsvInput,
    VueCsvErrors,
    VueCsvImport,
  },
```

- downlad csv file have link here **
- new thing appears is a map component automatically detected correct
- that's because of v model, can specify headers
- no know what happens with our csv and data etc
- v model tells us csv variable gets populated by our component and the mapping
- more visibility what happens csv variable, can do this iwth {{ variable }}, it will display
- pass data to a value


- display that value from v-model variable declaration (input binding) https://v2.vuejs.org/v2/guide/forms.html

```
    {{ csv }}
```

- need to register this model as a component again, in what is vue https://vuejs.org/guide/introduction.html, register syntax into data option
- after components
- need to initialize it, give it a value, so give it null

```javascript
  data() {
    return {
      csv: null,
    };
  },
```

- compnent automcaitlcally converted contents of csv into array now a javascript object, each object corresponds to a row in a csv. so have name, location, remove headers
- summary vmodel have model variable csv, to display use curly brackets but still need to register it so do it in script

TURNS CSV INTO JSON ARRAY 

- search "vue3 map" in google
- show leaflet library https://leafletjs.com/ we know leaflet already
- search "vue3 leaflet" in google see a component exists in npm js https://www.npmjs.com/package/@vue-leaflet/vue-leaflet, list of various subcomponents that can be used, summarize page
- start to add it into our application

MAP TIME using https://github.com/vue-leaflet/vue-leaflet

```
yarn add @vue-leaflet/vue-leaflet
```
- check package.json
- also need actual leaflet library, because vue-leaflet needs npm install leaflet or yarn add leaflet

- https://github.com/PaulLeCam/react-leaflet/issues/491

which modules from vue-leaflet do we need? at minimum? we need a map, tile layer, and markers

```javascript
import { LMap, LTileLayer, LMarker } from "@vue-leaflet/vue-leaflet";
import "leaflet/dist/leaflet.css";
```

- register in components

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

- finally use it in html template, syntax is vue based LMap or l-map both works

```html
    <l-map>
      <l-marker></l-marker>
      <l-tile-layer></l-tile-layer>
    </l-map>
```

https://vue2-leaflet.netlify.app/components/LMarker.html#demo

```html
    <l-map ref="myMap" style="height: 500px">
      <l-marker></l-marker>
      <l-tile-layer
        url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
      ></l-tile-layer
    ></l-map>
```

then add variables for map

```
zoom="2"
```

- now for tile layer variables. no need to add attribution parameter

```html
    <l-map style="height: 500px" :zoom="2">
      <l-marker></l-marker>
      <l-tile-layer
        url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
      ></l-tile-layer
    ></l-map>
```


```javascript
  data() {
    return {
      csv: null,
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
    };
  },
```

```html
    <l-map style="height: 500px" :zoom="2">
      <l-marker></l-marker>
      <l-tile-layer :url="url"></l-tile-layer
    ></l-map>
```

```html
    <l-map style="height: 500px" :zoom="2">
      <l-marker :lat-lng="markerLatLng"></l-marker>
      <l-tile-layer :url="url"></l-tile-layer
    ></l-map>
```

- in console have marker error

```javascript
  data() {
    return {
      csv: null,
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      markerLatLng: [51.504, -0.159],
    };
  },
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

PASCAL START HERE

RETRIEVE GEONAMES API

- http://www.geonames.org/export/web-services.html
- http://www.geonames.org/export/geonames-search.html
- JSON search
- http://api.geonames.org/searchJSON?q=london&maxRows=10&username=demo
- http://api.geonames.org/searchJSON?q=london&maxRows=10&username=pelagios
- http://api.geonames.org/searchJSON?q=london&maxRows=1&username=pelagios

- rate limited use pelagios isntead of demo
- returns json object

`yarn add axios`

`import axios from "axios";`


GET COORDINATES INTO JSON ARRAY

- use watch to trigger calls to api to return coordinates, every time csv variable changes. to do this use option vue provides called watch. watch keeps an eye on variables tell it to watch, if it changes perform an action
- keep an eye on csv value gets populated then grab array when it changes gets populated array so perform call to geonames api when we know that a csv was uploaded. to do this use a vue option called watch, allows us to keep an eye on variables used by vue component, do something when it changes. syntax is

https://vuejs.org/guide/essentials/watchers.html

```javascript
  watch: {
    csv(value) {
      console.log(value);
    },
  },

```

- comment out import axios so no error to test run
- check console, proxy appears in target open, see content of variable we watch
- keeps an eye on csv variable. everytime it changes, perform something with value some action. in this case console log the new value. so see wahwt happens when csv is loaded

- we know it works, now next step go through array iteratively, for each location perform call to geonames api get coordinates of each place. this is where we use axios

- to loop through array use forEach https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach

```javascript
  watch: {
    csv(value) {
      console.log(value);
      value.forEach((element) => {
        console.log(element);
      });
    },
  },
```

- before output array now output each line of javascript array as an object

```javascript
  watch: {
    csv(value) {
      console.log(value);
      value.forEach((element) => {
        console.log(element.location);
      });
    },
  },
```

output string. getting close to getting data. now use axios to send location to geonames geolocation server to get coordinates

uncomment axios to later compile, will not work yet until you use it

lookup how to get axios get request in browser
https://github.com/axios/axios#note-commonjs-usage

call axios.get, at the url geonames service, then use result of this call to perform an action , in our case markers for latitude and longitude. in case of an error catch and display errors in console.

```javascript
  watch: {
    csv(value) {
      console.log(value);
      value.forEach((element) => {
        axios.get(`http://api.geonames.org/searchJSON?q=london&maxRows=1&username=pelagios`)
        .then(function(response){
          console.log(response);
        })
        console.log(element.location);
      });
    },
  },
```

use back ticks for string interpolation for url in axios need these characters

can see result of call status geonames api data part of object, lat and lng has coordinates

replace london hardcoded variable with element location in foreach, then explore response to get both lat and lng with variabels later

use string interpolation syntax replace london with variable location, then get lat and lng out in response object geonames, grab out
const constant in this scope

```javascript
 watch: {
    csv(value) {
      console.log(value);
      value.forEach((element) => {
        axios
          .get(
            `http://api.geonames.org/searchJSON?q=${element.location}&maxRows=1&username=pelagios`
          )
          .then(function (response) {
            console.log(response);
            const geonamesData = response.data.geonames[0];
            const latitude = geonamesData["lat"];
            const longitude = geonamesData["lng"];
            console.log(latitude);
            console.log(longitude);
          });
        console.log(element.location);
      });
    },
  },
```

check console have latitude and longitude output coordinates we are now able to grab

all that's left to do now is to create markers for the coordinates we have now

l-marker tag, specified latitude and longitude in data variable registered. now replace a hardcoded marker with array of markers lat and long pairs for geonames api

first step to say markers variable wanted

```javascript
 watch: {
    csv(value) {
      console.log(value);
      value.forEach((element) => {
        axios
          .get(
            `http://api.geonames.org/searchJSON?q=${element.location}&maxRows=1&username=pelagios`
          )
          .then((response) => {
            console.log(response);
            const geonamesData = response.data.geonames[0];
            const latitude = geonamesData["lat"];
            const longitude = geonamesData["lng"];
            console.log(latitude);
            console.log(longitude);
            this.markers.push([latitude, longitude]);

          });
        console.log(element.location);
      });
    },
  },
```

to reference a variable in componenet uneed to use this

now a variable in component can output it

es6 then(function (response) { vs (response) arrow function

```
    <br /><br />
    {{ csv }}
    <br/>
    {{ markers }}

```

markers get populated, display in browser, display in console

now use array of markers to place markers on map

temporary hardcoded marker using markerlatlng defined in data option, instead we have to iterate marker array foreach element in marker create marker lmarker tag
to do this use another functaionlity vue provides, v for iterate in html

vue needs a key, set markers, should palce markers in map

```html
    <l-map style="height: 500px" :zoom="2">
      <l-marker
        v-for="marker in markers"
        :lat-lng="marker"
        :key="marker"
      ></l-marker>
      <l-tile-layer :url="url"></l-tile-layer
    ></l-map>

```

clean up code a bit

go back into data option remove       markerLatLng: [51.504, -0.159],

remove console. logs

```
 watch: {
    csv(value) {
      value.forEach((element) => {
        axios
          .get(
            `http://api.geonames.org/searchJSON?q=${element.location}&maxRows=1&username=pelagios`
          )
          .then((response) => {
            const geonamesData = response.data.geonames[0];
            const latitude = geonamesData["lat"];
            const longitude = geonamesData["lng"];
            this.markers.push([latitude, longitude]);
          });
      });
    },
  },
```

NO MORE END HERE STOP

make pretty scrolling better zooming

automatically frames markers

add l-map ref

```
<l-map
      ref="myMap"
      style="height: 500px"
      :zoom="2"
      @ready="doSomethingOnReady"
    >
      <l-marker
        v-for="marker in markers"
        :lat-lng="marker"
        :key="marker"
      ></l-marker>
      <l-tile-layer :url="url"></l-tile-layer
    ></l-map>
```

get access to leaflet map object to do stuff on

```
  data() {
    return {
      csv: null,
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      markers: [],
      map: null,
    };
  },

```

```
  methods: {
    doSomethingOnReady() {
      this.map = this.$refs.myMap.leafletObject;
      //this.map.fitBounds(this.markers);
    },
  },
```

trigger zoom out do it in the watcher after see marker array complete

use special fly to bounds in leaflet https://www.tabnine.com/code/javascript/functions/leaflet/Map/flyToBounds

when markers are as long as csv array meaning when array is completed go through loop, fly to bounds

```
  watch: {
    csv(value) {
      value.forEach((element) => {
        axios
          .get(
            `http://api.geonames.org/searchJSON?q=${element.location}&maxRows=1&username=pelagios`
          )
          .then((response) => {
            const geonamesData = response.data.geonames[0];
            const latitude = geonamesData["lat"];
            const longitude = geonamesData["lng"];
            this.markers.push([latitude, longitude]);
            if (this.markers.length == this.csv.length) {
              this.map.flyToBounds(this.markers, { padding: [100, 100] });
            }
          });
      });
    },
  },

```
