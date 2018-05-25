<template>
  <div id="app">
    <search-results msg="Equella Searching Demo" searchResultTypes="mini courses" baseUrl="https://equella.unicon.net/demo-oa18-up-eq-integ" collectionId="1ffa9507-4313-4f4f-9f6c-b923a897c515" resultIcon="_THUMBS/promo.jpeg_PREVIEW_.jpeg" resultLandingPage="index.html"/>
  </div>
</template>

<script>
import SearchResults from "./components/SearchResults.vue";

export default {
  name: "app",
  components: {
    "search-results": SearchResults
  }
};
</script>

<style>
</style>
