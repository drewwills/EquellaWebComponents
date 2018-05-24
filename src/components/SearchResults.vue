<template>
  <div class="container search-results">
    <img src="https://equella.unicon.net/demo-oa18-up-eq-integ/s/ewc-logo" class="img-rounded pull-right" alt="EQUELLA Logo" height="80">
    <h1>{{ msg }}</h1>
    <p>
      Found the following items in Equella.
    </p>
    <ul class="list-group">
      <li class="list-group-item" v-for="item in items" :key="item.uuid">
        <a v-bind:href="item.url"><strong>{{ item.name }}</strong></a>
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
      const response = await fetch('https://equella.unicon.net/demo-oa18-up-eq-integ/api/search/?start=0&length=10&collections=812ee0bb-b53e-4a84-87db-0e328b374494&order=name&reverse=false&info=metadata&status=LIVE');
      if (!response.ok) {
        throw new Error(response.statusText);
      }
      const data = await response.json();
      this.items = data.results.map(item => {
        let xml = new DOMParser().parseFromString(item.metadata, 'application/xml');
        return {
          uuid: item.uuid,
          name: xml.getElementsByTagName('name')[0].textContent,
          description: xml.getElementsByTagName('description')[0].textContent,
          url: item.links.view
        };
      });
      // eslint-disable-next-line
      console.log(this.items);
    } catch (err) {
      // TODO: User-appropriate feedback
      // eslint-disable-next-line
      console.error(err);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.search-results {
  padding: 2rem;
}
</style>
