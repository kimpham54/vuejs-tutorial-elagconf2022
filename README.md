# vuejs-tutorial-elagconf2022

## Tutorial Steps
1. go through what to install, node, yarn, vue cli
2. explain the various options when creating a new project, (briefly explain frameworks like nuxt, etc)
3. go through the folders that get created when you initialise a vue project, but also note that vue can be incorporated into any project using a CDN for example
4. talk briefly about vscode and linting, plugins to make dev easier
5. start going through the first default HelloWorld.vue component, and start to modify it to do what we want to do: something that ingests a csv, gets the coords, and show them on a leaflet map
6. also show how you can debug and see what's going on using the vue console tool and the normal console tool
7. FIRST COMPONENT show how to import a package that will process the csv
8. add tailwind.css into the mix for a bit of styling
9. SECOND COMPONENT show how to import the vue3 leaflet component
10. LAST PIECE: import axios to tie it all together: show how to go through the csv import loop, call geonames, and create the markers that will be displayed on the map
11. if time left: add more functionalities, such as popups on markers, Q&A
    
   


## Project setup
```
install node global
install yarn global
yarn install in the directory

DEPENDENCIES
yarn add vue-csv-import
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
