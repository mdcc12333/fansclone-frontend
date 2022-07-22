<template>
  <div class="d-flex flex-column content landing">
    <div v-if="!isMobile()">
      <AppHeader />
    </div>
    <div v-else>
      <AppHeaderMobile />
    </div>
    <b-container>
      <!-- <h1>discovery</h1> -->
      <b-row>
        <b-col
            :lg="
            $route.name == 'subscriptions' ||
            $route.name == 'list' ||
            $route.name == 'messages' ||
            $route.name == 'chat'
              ? 9
              : 6
          "
            md="9"
            sm="9"
            cols="12"
            class="content offset-md-3"
        >
          <router-view></router-view>
          <div class="spinner" v-if="spinner">
            <b-spinner variant="secondary" />
          </div>
        </b-col>
        <b-col md="3" class="d-none d-lg-block">
          <router-view name="side" class="sticky-top"></router-view>
        </b-col>
      </b-row>
    </b-container>
    <!-- <app-menu /> -->

  </div>
</template>
<script>
// import AppMenu from "../AppMenu.vue";
import AppHeader from "../AppHeader.vue";
import AppHeaderMobile from "../AppHeaderMobile.vue";

export default {
  computed: {
    spinner() {
      return this.$store.state.spinner == true;
    },
    currentUser() {
      return this.$store.state.currentUser;
    },
    updates() {
      return this.$store.state.updates;
    },
  },
  methods: {
    isMobile() {
      if(/Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent)) {
        return true
      } else {
        return false
      }
    },
  },
  components: { AppHeader, AppHeaderMobile },
};
</script>
