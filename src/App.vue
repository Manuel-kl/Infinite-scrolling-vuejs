<template>
  <home-component />
</template>
<script setup>
import HomeComponent from "./components/HomeComponent.vue";
</script>
<style lang="css">
/* For WebKit-based browsers (Chrome, Safari) */
::-webkit-scrollbar {
  width: 0; /* Hide horizontal scrollbar */
  height: 0; /* Hide vertical scrollbar */
}

/* For Firefox and other browsers */
html {
  scrollbar-width: none;
}
</style>