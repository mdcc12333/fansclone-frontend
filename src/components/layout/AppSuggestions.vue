<template>
  <b-row class="sticky-top top-models">
      <div class="d-flex align-items-center w-100 px-3 border-gray">
        <h5 class="p-0 my-3 flex-grow-1 top-models-header">
          {{ $t("general.suggestions") }}
        </h5>
        <!-- <b-link @click.prevent="slideLeft" class="mr-2">
          <i class="bi-arrow-left-circle" />
        </b-link>
        <b-link @click.prevent="slideRight">
          <i class="bi-arrow-right-circle" />
        </b-link> -->
      </div>
      <div ref="swiper" class="swiper w-100 overflow-hidden px-3 pt-19 border-gray">
        <div class="swiper-wrapper">
          <div
            class="swiper-slide d-block"
            v-for="(item, key) in chunkedArr"
            :key="key"
          >
            <ui-suggestion
              :user="user"
              v-for="(user, k) in item"
              :key="k"
              class="mb-3"
            />
          </div>
        </div>
      </div>
  </b-row>
</template>
<script>
import { Swiper } from "swiper";
import "swiper/swiper-bundle.css";
import User from "../models/User";
import UiSuggestion from "../ui/UiSuggestion.vue";
export default {
  components: { UiSuggestion },
  data: function () {
    return {
      users: [],
      swiper: null,
    };
  },
  mounted() {
    this.swiper = new Swiper(this.$refs.swiper, {
      // Optional parameters
      direction: "horizontal",
    });
    this.loadSuggestions();
  },
  computed: {
    chunkedArr() {
      const result = [];
      for (let i = 0; i < this.users.length; i += 5)
        result.push(this.users.slice(i, i + 5));
      return result;
    },
  },
  methods: {
    loadSuggestions() {
      this.$get(
        "/users",
        (data) => {
          let users = [];
          for (let obj of data.users) {
            users.push(new User(obj));
          }
          this.users = users;
          this.$nextTick(function () {
            this.swiper.update();
          });
        },
        (errors) => {
          console.log(errors);
        }
      );
    },
    slideLeft() {
      this.swiper.slidePrev();
    },
    slideRight() {
      this.swiper.slideNext();
    },
  },
};
</script>

<style>
.top-models{
  margin: 17px -90px 0 5px;
}
.top-models-header{
  font-weight: 700;
  font-size: 17px;
}
.pt-19{
  padding-top: 19px;
}
</style>
