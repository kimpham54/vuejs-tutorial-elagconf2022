# vuejs-tutorial-elagconf2022

Welcome to our ELAG 2022 Vue for beginners code along!

## What we will build

<img width="1006" alt="Screenshot 2022-05-06 at 13 26 13" src="https://user-images.githubusercontent.com/28725039/167122841-5f9d0722-3b8b-4188-ba18-a8890ad50fb2.png">

Our goal is to build a simple application that allows users to upload a CSV document containing a list of museums and the city they are in.

Once the CSV has been uploaded, our application will call an external geolocation service to obtain the coordinates of each of these town. 

Finally, once we have all the coordinates, we will place markers for each of the museums on a map.

## Preparation

To follow along, you will need to:

- install a code editor such as VSCode or Sublime
- install [node.js](https://nodejs.org/en/download/)
- (optional) install [yarn](https://classic.yarnpkg.com/lang/en/docs/install/#mac-stable)
- install [vue-cli](https://cli.vuejs.org/guide/installation.html)

## Tutorial Steps
We will create this application from scratch (from an empty project) and implement the project live. To prepare, participants should ideally install node, yarn, and vue cli. It would also be quite useful to have installed a code editor such as VSCode or Sublime.


1. go through what to install, node, yarn, vue cli (ideally should be done already by all participants) (KIM)
2. explain the various options when creating a new project, (briefly explain frameworks like nuxt, etc) (PASCAL)
3. go through the folders that get created when you initialise a vue project, but also note that vue can be incorporated into any project using a CDN for example (PASCAL)
4. talk briefly about vscode and linting, plugins to make dev easier (KIM)
5. explanation of basic Vue.js concepts (PASCAL)
   1. What is a component
      1. what are the parts of a component: template, component definition, styling
      2. what are the parts and hooks of the component definition we will be using:
         1. data
         2. methods
         3. watch
         4. beforeMount hook
6. start going through the first default HelloWorld.vue component, and start to modify it to do what we want to do: 
   1.  something that ingests a csv (KIM)
   2. gets the coords for each of the rows in the CSV from a remote geolocation service (PASCAL)
   3. show markers based on these coordinates on the map map (PASCAL)
9. also show how you can debug and see what's going on using the vue console tool and the normal console tool (PASCAL)

## Our application's components

1. FIRST COMPONENT show how to import a package that will process the csv. The example csv for this tutorial can be found there: https://docs.google.com/spreadsheets/d/1ByRVJa_B0Zf0_IlQPki94OsKfodlNZbJ4wtOj4XLRgM/edit?usp=sharing (KIM)

2. SECOND COMPONENT show how to import the vue3 leaflet component (PASCAL), and add it to our application (PASCAL)
3. LAST PIECE: import axios to tie it all together: show how to go through the csv import loop, call geonames, and create the markers that will be displayed on the map (PASCAL)

4. if there is time left: add more functionalities, such as popups on markers, (PASCAL)
5.  add tailwind.css into the mix for a bit of styling (PASCAL & KIM)
6.  Q&A (PASCAL)
   
## Project setup
```
vue create elag_tutorial
cd elag_tutorial
yarn

DEPENDENCIES
yarn add vue-csv-import
(in index.html header: tailwind.css CDN <script src="https://cdn.tailwindcss.com"></script>)
yarn add leaflet
yarn add @vue-leaflet/vue-leaflet
yarn add axios
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
