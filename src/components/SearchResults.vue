<template>
  <div class="search-results">
    <div class="search-header clearfix">
      <img id="equellaLogo" v-bind:src="baseUrl + '/s/ewc-logo'" class="rounded float-right" alt="EQUELLA Logo" />
      <h1>
        {{ msg }}
      </h1>
      <p>
        Found the following {{items.length}} {{ searchResultTypes }} in Equella.
      </p>
    </div>
    <div class="search-items list-group">
      <div class="list-group-item media" v-for="item in items" :key="item.uuid">
        <div class="mr-3">
          <img v-bind:src="item.url + resultIcon" class="media-object" style="width:180px">
        </div>
        <div class="media-body">
          <h4 class="media-heading">
            <a v-bind:href="item.url + resultLandingPage">
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

<!-- a minimal scoped version of bootstrap -->
<style lang="scss">
// core bootstrap framework
@import "../../node_modules/bootstrap/scss/functions.scss";
@import "../../node_modules/bootstrap/scss/variables.scss";
@import "../../node_modules/bootstrap/scss/mixins.scss";

// bootstrap styles needed by page
@import "../../node_modules/bootstrap/scss/type.scss";
@import "../../node_modules/bootstrap/scss/utilities.scss";
@import "../../node_modules/bootstrap/scss/list-group.scss";
@import "../../node_modules/bootstrap/scss/media.scss";

.search-results {
  padding: $spacer * 0.25;
}

#equellaLogo {
  height: $spacer * 8;
}

.search-header p {
  font-size: $h3-font-size;
}

.search-items {
  margin-top: $spacer;
}

.search-items p {
  font-size: $h4-font-size;
}
</style>
