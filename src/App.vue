<template>
  <div class="grid gap-3">
    <CSVPlaceFinder />
  </div>
</template>

<script>
import CSVPlaceFinder from "./components/CSVPlaceFinder.vue";

export default {
  name: "App",
  components: {
    CSVPlaceFinder,
  },
};
</script>
