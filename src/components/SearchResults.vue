<template>
  <div class="search-results">
    <div class="search-header clearfix">
      <img id="equellaLogo" src="https://equella.unicon.net/demo-oa18-up-eq-integ/s/ewc-logo" class="img-rounded pull-right" alt="EQUELLA Logo" />
      <h1>
        {{ msg }}
      </h1>
      <p>
        Found the following {{items.length}} {{ searchResultTypes }} in Equella.
      </p>
    </div>
    <div class="search-items list-group">
      <div class="list-group-item media" v-for="item in items" :key="item.uuid">
        <div class="media-left">
          <img v-bind:src="item.url + resultIcon" class="media-object" style="width:180px">
        </div>
        <div class="media-body">
          <h4 class="media-heading">
            <a v-bind:href="item.url + '/{{ resultLandingPage }}'">
              {{ item.name }}
            </a>
          </h4>
          <p>
            {{ item.description }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<!-- a minimal scoped version of bootstrap -->
<style lang="less" scoped>
// core bootstrap framework
@import "../../node_modules/bootstrap/less/variables.less";
@import "../../node_modules/bootstrap/less/mixins.less";

// styles needed by page
@import "../../node_modules/bootstrap/less/type.less";
@import "../../node_modules/bootstrap/less/utilities.less";
@import "../../node_modules/bootstrap/less/list-group.less";
@import "../../node_modules/bootstrap/less/media.less";
</style>

<script>
export default {
  props: {
    msg: String,
    baseUrl: String,
    collectionId: String,
    searchResultTypes: String,
    resultIcon: String,
    resultLandingPage: String
  },
  data() {
    return {
      items: []
    };
  },
  async created() {
    try {
      const response = await fetch(
        `${this.baseUrl}/api/search/?start=0&length=10&collections=${
          this.collectionId
        }&order=name&reverse=false&info=basic&status=LIVE`
      );
      if (!response.ok) {
        throw new Error(response.statusText);
      }
      const data = await response.json();
      this.items = data.results.map(
        ({ uuid, name, description, links: { view: url } }) => ({
          uuid,
          name,
          description,
          url
        })
      );
      // eslint-disable-next-line
      console.log(this.items);
    } catch (err) {
      // TODO: User-appropriate feedback
      // eslint-disable-next-line
      console.error(err);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.search-results {
  padding: 2rem;
}
#equellaLogo {
  height: 8rem;
}
.search-header p {
  font-size: 150%;
}
.search-items {
  margin-top: 1rem;
}
.search-items p {
  font-size: 125%;
}
</style>
