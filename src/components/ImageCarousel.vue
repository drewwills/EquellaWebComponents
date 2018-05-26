<template>
  <div>
    <b-carousel id="carousel1"
                controls
                indicators
                background="#ababab"
                :interval="4000"
                img-width="1280"
                img-height="256"
                v-model="slide"
                @sliding-start="onSlideStart"
                @sliding-end="onSlideEnd"
    >

      <b-carousel-slide v-for="item in items" :key="item.uuid" v-bind:caption="item.name"
                        v-bind:text="item.description"
                        v-bind:img-src="item.imageUrl"
      ></b-carousel-slide>


    </b-carousel>


    <p class="mt-4">
      Slide #: {{ slide }}<br>
      Sliding: {{ sliding }}
    </p>

  </div>
</template>

<script>
import carousel from "bootstrap-vue/es/components/carousel/carousel";
import slide from "bootstrap-vue/es/components/carousel/carousel-slide";

export default {
  props: {
    baseUrl: String,
    searchTag: String
  },
  components: {
    "b-carousel": carousel,
    "b-carousel-slide": slide
  },
  data() {
    return {
      slide: 0,
      sliding: null,
      items: []
    };
  },
  methods: {
    onSlideStart(slide) {
      console.log(slide);
      this.sliding = true;
    },
    onSlideEnd(slide) {
      console.log(slide);
      this.sliding = false;
    }
  },
  async created() {
    try {
      const response = await fetch(
        `${this.baseUrl}/api/search/?start=0&length=10&reverse=false&where=%2Fxml%2Fmetadata%2Ftags%2Ftag%20%3D%20'${
          this.searchTag
        }'&info=all&showall=false&status=LIVE`
      );
      if (!response.ok) {
        throw new Error(response.statusText);
      }
      const data = await response.json();
      this.items = data.results.map(
        ({ uuid, name, description, attachments }) => ({
          uuid,
          name,
          description,
          imageUrl:attachments[0].links.view
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
@import "../../node_modules/bootstrap/scss/type";
@import "../../node_modules/bootstrap/scss/images";
@import "../../node_modules/bootstrap/scss/carousel";
@import "../../node_modules/bootstrap/scss/utilities";

.carousel-caption {
  background: rgba(0, 0, 0, 0.5);
}
</style>
