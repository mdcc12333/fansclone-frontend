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
        <router-view></router-view>
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
