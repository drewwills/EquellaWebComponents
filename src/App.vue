<template>
  <div id="app">
    <search-results msg="Equella Search Results"/>
  </div>
</template>

<script>
import SearchResults from './components/SearchResults.vue'

export default {
  name: 'app',
  components: {
    'search-results': SearchResults
  }
}
</script>

<style>
</style>
