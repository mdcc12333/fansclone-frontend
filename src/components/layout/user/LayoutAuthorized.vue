<template>
  <div>
    <div v-if="!isMobile()">
      <AppHeader />
    </div>
    <div v-else>
      <AppHeaderMobile />
    </div>
    <b-container>
      <b-row>
        <b-col lg="3"></b-col>
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
          class="content"
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
    <b-nav
      class="
        position-fixed
        footer
        w-100
        bg-white
        border-top
        justify-content-between
        align-items-center
        d-flex d-sm-none
      "
    >
      <b-nav-item to="/" exact>
        <div class="icon d-inline-block"><i class="bi-house" /></div>
      </b-nav-item>
      <b-nav-item to="/notifications">
        <div class="icon d-inline-block">
          <div class="icon d-inline-block"><i class="bi-bell" /></div>
          <div class="icon d-inline-block">
            <i class="bi-dot badge" v-if="updates.notifications > 0" />
          </div>
        </div>
      </b-nav-item>
      <b-nav-item to="/posts/create">
        <div class="icon d-inline-block"><i class="bi-plus-circle" /></div>
      </b-nav-item>
      <b-nav-item to="/messages">
        <div class="icon d-inline-block">
          <div class="icon d-inline-block"><i class="bi-chat" /></div>
          <div class="icon d-inline-block">
            <i class="bi-dot bagde" v-if="updates.messages > 0" />
          </div>
        </div>
      </b-nav-item>
      <b-nav-item v-b-toggle.sidebar>
        <b-avatar
          :text="currentUser.initials"
          :src="currentUser.avatar"
          class="bg-secondary text-white"
        />
      </b-nav-item>
    </b-nav>
    <app-menu />
  </div>
</template>
<style scoped lang="scss">
@import "~@/assets/scss/_variables.scss";
@include media-breakpoint-down(md) {
  .nav {
    text-align: center;
  }
}
@include media-breakpoint-up(lg) {
  .nav {
    text-align: left;
  }
}
a.nav-link {
  font-size: 1.2rem;
  line-height: 1.4rem;
  padding: 0.75rem 1rem;
  vertical-align: middle;
}
.icon {
  position: relative;
  font-size: 1.4rem;
  line-height: 1.4rem;
  .bi-dot {
    position: absolute;
    bottom: 0;
    right: 0;
    font-size: 3.5rem;
    line-height: 1;
    margin: -1.8rem;
  }
}
.footer {
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 1001;
}
.spinner {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 10000;
}
</style>
<script>
import AppMenu from "../AppMenu.vue";
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
  components: { AppMenu, AppHeader, AppHeaderMobile },
};
</script>
