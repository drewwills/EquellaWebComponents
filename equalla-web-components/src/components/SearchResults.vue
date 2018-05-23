<template>
  <div class="search-results">
    <h1>{{ msg }}</h1>
    <p>
      For guide and recipes on how to configure / customize this project,<br>
      check out the
      <a href="https://github.com/vuejs/vue-cli/tree/dev/docs" target="_blank">vue-cli documentation</a>.
    </p>
    <ul>
      <li v-for="item in items" :key="item.uuid">
        <a v-bind:href="item.url">{{ item.name }}</a>
        <p>{{ item.description }}</p>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    msg: String
  },
  data() {
    return {
      items: []
    }
  },
  async created() {
    try {
      const response = await fetch('/sample/search.json');
      if (!response.ok) {
        throw new Error(response.statusText);
      }
      const data = await response.json();
      for (const item of data) {
        let xml = new DOMParser().parseFromString(item.metadata, 'application/xml');
        this.items.push({
          uuid: item.uuid,
          name: xml.getElementsByTagName('name')[0].textContent,
          description: xml.getElementsByTagName('description')[0].textContent,
          url: item.links.view
        });
      }
      console.log(data);
      console.log(this.items);
    } catch (err) {
      // TODO: User-appropriate feedback
      console.error(err);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
