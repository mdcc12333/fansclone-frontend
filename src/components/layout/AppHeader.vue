<template>
  <header class="w-full bg-main py-2">
    <nav class="navbar navbar-expand-md navbar-light bg-light">
        <div class="w-100 order-1 order-md-0">
          <a class="navbar-brand mx-auto" href="#"><img src="../../assets/logo.svg"></a>
          <!-- <button class="navbar-toggler" type="button" data-toggle="collapse" data-target=".dual-collapse2">
            <i class="bi-list" />
          </button> -->

        </div>
        <div class="mx-auto order-0 collapse navbar-collapse">
          <ul class="navbar-nav mr-auto">
            <b-nav-item class="mr-3" to="/">
              <i class="bi-house" style="font-size: 2rem;" /> {{ $t("general.home") }}
            </b-nav-item>
            <b-nav-item class="mr-3" to="/subscribed">
              <i class="bi-lock" style="font-size: 2rem;" /> {{ $t("general.subscribed") }}
            </b-nav-item>
            <b-nav-item class="mr-3" to="/posts/create">
              <i class="bi-plus-circle" style="font-size: 3rem; color: #CE1126;" />
            </b-nav-item>
            <b-nav-item class="mr-3" to="/messages">
              <i class="bi-chat" style="font-size: 2rem;" /> {{ $t("general.messages") }}
            </b-nav-item>
            <b-nav-item class="mr-3" v-if="currentUser" :to="currentUser.username">
              <i class="bi-person" style="font-size: 2rem;" /> {{ $t("general.profile") }}
            </b-nav-item>
            <!-- <li class="nav-item active">
              <a href="#" class="nav-link d-flex flex-column">
                <i class="bi-house" />
                <span class="d-none d-sm-inline">Home</span>
              </a>
            </li> -->
            <!-- <li class="nav-item active">
              <a href="#" class="nav-link d-flex flex-column">
                <i class="bi-lock" />
                <span class="d-none d-sm-inline">Subscribed</span>
              </a>
            </li>
            <li class="nav-item active">
              <a href="#" class="nav-link d-flex flex-column">
                <i class="bi-plus-circle" />
              </a>
            </li>
            <li class="nav-item active">
              <a href="#" class="nav-link d-flex flex-column">
                <i class="bi-chat" />
                <span class="d-none d-sm-inline">Messages</span>
              </a>
            </li>
            <li class="nav-item active">
              <a href="#" class="nav-link d-flex flex-column">
                <i class="bi-person" />
                <span class="d-none d-sm-inline">Profile</span>
              </a>
            </li> -->
          </ul>
        </div>
        <div class="navbar-nav w-100 order-1">
          <ul class="navbar-nav ml-auto d-flex flex-row">
            <li class="nav-item mr-3">
              <b-button class="btn-primary" v-b-modal="'signin-modal'">Sign in</b-button>
            </li>
            <li class="nav-item">
              <b-button class="btn-primary" v-b-modal="'signup-modal'">Sign up</b-button>
            </li>
            <b-nav-item to="/notifications">
              <div class="icon d-inline-block">
                <i class="bi-bell" />
                <!-- <i class="bi-dot" v-if="updates.notifications > 0" /> -->
              </div>
            </b-nav-item>
            <!-- <a href="#" class="nav-link d-flex flex-column">
              <i class="bi-bell" />
            </a> -->
            <a class="nav-link d-flex flex-column" v-b-toggle.sidebar>
              <i class="bi-list" />
            </a>
          </ul>
        </div>
    </nav>
    <!-- <b-sidebar id="sidebar-1" title="Sidebar" shadow>
      <b-col lg="3" md="2" sm="2" class="d-none d-sm-block">
        <b-nav vertical class="sticky-top">
          <b-nav-item v-b-toggle.sidebar>
            <b-avatar
              :text="currentUser.initials"
              :src="currentUser.avatar"
              class="bg-secondary text-white"
            />
          </b-nav-item>
          <b-nav-item to="/" exact>
            <div class="icon d-inline-block"><i class="bi-house" /></div>
            <span class="d-none d-lg-inline ml-3">{{
              $t("general.home")
            }}</span>
          </b-nav-item>
          <b-nav-item to="/notifications">
            <div class="icon d-inline-block">
              <i class="bi-bell" />
              <i class="bi-dot" v-if="updates.notifications > 0" />
            </div>
            <span class="d-none d-lg-inline ml-3">{{
              $t("general.notifications")
            }}</span>
          </b-nav-item>
          <b-nav-item to="/messages">
            <div class="icon d-inline-block">
              <i class="bi-chat" />
              <i class="bi-dot" v-if="updates.messages > 0" />
            </div>
            <span class="d-none d-lg-inline ml-3">{{
              $t("general.messages")
            }}</span>
          </b-nav-item>
          <b-nav-item to="/bookmarks" exact>
            <div class="icon d-inline-block"><i class="bi-bookmark" /></div>
            <span class="d-none d-lg-inline ml-3">{{
              $t("general.bookmarks")
            }}</span>
          </b-nav-item>
          <b-nav-item to="/lists">
            <div class="icon d-inline-block"><i class="bi-list" /></div>
            <span class="d-none d-lg-inline ml-3">{{
              $t("general.lists")
            }}</span>
          </b-nav-item>
          <b-nav-item to="/subscriptions">
            <div class="icon d-inline-block"><i class="bi-heart" /></div>
            <span class="d-none d-lg-inline ml-3">{{
              $t("general.subscriptions")
            }}</span>
          </b-nav-item>
          <b-nav-item :to="currentUser.url" exact>
            <div class="icon d-inline-block"><i class="bi-person" /></div>
            <span class="d-none d-lg-inline ml-3">{{
              $t("general.my-profile")
            }}</span>
          </b-nav-item>
          <b-nav-item v-b-toggle.sidebar>
            <div class="icon d-inline-block"><i class="bi-three-dots" /></div>
            <span class="d-none d-lg-inline ml-3">{{
              $t("general.more")
            }}</span>
          </b-nav-item>
          <b-nav-item>
            <b-button
              class="w-100 d-lg-block d-md-none d-sm-none"
              :to="currentUser.isCreator ? '/posts/create' : '/payouts'"
              variant="primary"
              >{{ $t("general.new-post") }}</b-button
            >
            <b-button
              class="w-100 d-md-block d-sm-block d-lg-none p-2"
              :to="currentUser.isCreator ? '/posts/create' : '/payouts'"
              variant="primary"
            >
              <div class="icon d-inline-block">
                <i class="bi-plus" style="font-size: 150%; line-height: 1" />
              </div>
            </b-button>
          </b-nav-item>
        </b-nav>
      </b-col>
    </b-sidebar> -->
    <b-modal id="signin-modal" size="md" hide-footer>
      <b-container fluid>
        <ModalLogin
          v-show="1==1"
          @close="closeModal"
        />
      </b-container>
    </b-modal>
    <b-modal id="signup-modal" size="md" hide-footer>
      <b-container fluid>
        <ModalSignup
          v-show="1==1"
          @close="closeModal"
        />
      </b-container>
    </b-modal>
    <!-- <app-suggestions-insta /> -->
  </header>
</template>
<script>
import ModalLogin from '../ModalLogin.vue';
import ModalSignup from '../ModalSignup.vue';
// import AppSuggestionsInsta from "./AppSuggestionsInsta.vue";

export default{
  components: {
    ModalLogin, ModalSignup,
  },
  computed: {
    currentUser() {
      return this.$store.state.currentUser;
    },
  },
  data() {
    return {
      isModalVisible: false,
    };
  },
  methods: {
    showModal() {
      this.isModalVisible = true;
    },
    closeModal() {
      this.isModalVisible = false;
    }
  }
};

</script>
<style scoped>
  .nav-item a{
    text-align: center;
  }
  .btn-primary{
    background: #002D64;
    border-radius: 8px;
  }

</style>
