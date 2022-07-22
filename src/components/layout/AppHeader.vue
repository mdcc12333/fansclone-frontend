<template>
  <header class="w-full bg-main">
    <nav class="navbar navbar-expand-md navbar-light">
      <div class="w-100 order-1 order-md-0">
        <a class="navbar-brand mx-auto" href="#"><img src="../../assets/logo.svg" alt="logo" class="logo"></a>
      </div>
      <div class="mx-auto order-0 collapse navbar-collapse">
        <ul class="navbar-nav mr-auto">
          <b-nav-item class="mr-3" to="/">
            <img v-if="currentRoute('home')" src="../../assets/icons/home-filled.svg" alt="home" class="icon">
            <img v-else src="../../assets/icons/home.svg" alt="home" class="icon">
            {{ $t("general.home") }}
          </b-nav-item>
          <b-nav-item class="mr-3" to="/subscriptions">
            <img v-if="currentRoute('subscriptions')" src="../../assets/icons/lock-filled.svg" alt="home" class="icon">
            <img v-else src="../../assets/icons/lock.svg" alt="home" class="icon">
            {{ $t("general.subscribed") }}
          </b-nav-item>
          <b-nav-item class="mr-3" to="/posts/create">
            <img src="../../assets/icons/add-circle.svg" alt="add" class="circle-add">
          </b-nav-item>
          <b-nav-item class="mr-3" to="/messages">
            <img v-if="currentRoute('messages')" src="../../assets/icons/messages-filled.svg" alt="home" class="icon">
            <img v-else src="../../assets/icons/messages.svg" alt="home" class="icon">
            {{ $t("general.messages") }}
          </b-nav-item>
          <b-nav-item class="mr-3" v-if="currentUser" :to="currentUser.username">
            <img v-if="currentRoute('profile')" src="../../assets/icons/profile-filled.svg" alt="profile" class="icon">
            <img v-else src="../../assets/icons/profile.svg" alt="profile" class="icon">
            {{ $t("general.profile") }}
          </b-nav-item>
          <b-nav-item class="mr-3" v-else v-b-modal="'signin-modal'">
            <img v-if="currentRoute('profile')" src="../../assets/icons/profile-filled.svg" alt="profile" class="icon">
            <img v-else src="../../assets/icons/profile.svg" alt="profile" class="icon">
            {{ $t("general.profile") }}
          </b-nav-item>
        </ul>
      </div>
      <div class="navbar-nav w-100 order-1">
        <ul class="navbar-nav ml-auto d-flex flex-row">
          <li class="nav-item mr-3">
            <b-button v-if="!currentUser" class="btn-primary" v-b-modal="'signin-modal'">Sign in</b-button>
          </li>
          <li class="nav-item">
            <b-button v-if="!currentUser" class="btn-primary" v-b-modal="'signup-modal'">Sign up</b-button>
          </li>
          <b-nav-item to="/notifications">
            <div class="icon d-inline-block">
              <img src="../../assets/icons/bell.svg" alt="profile" class="icon">
            </div>
          </b-nav-item>
          <a class="nav-link d-flex flex-column" v-b-toggle.sidebar>
            <img src="../../assets/icons/line.svg" alt="profile" class="icon">
          </a>
        </ul>
      </div>
    </nav>
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
  </header>
</template>
<script>
import ModalLogin from '../ModalLogin.vue';
import ModalSignup from '../ModalSignup.vue';

export default {
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
    },
    currentRoute(name) {
      return this.$route.name === name
    }
  }
};

</script>
<style scoped>
.nav-item a {
  text-align: center;
}

.btn-primary {
  background: #002D64;
  border-radius: 8px;
}

.circle-add {
  width: 57px;
}

.icon {
  height: 30px;
  cursor: pointer;
}

.navbar-light {
  border-bottom: 2px solid #E3E3E3;
}

.logo {
  margin-bottom: 4px;
}

.btn {
  height: 43px;
  font-size: 15px;
  color: #E6E6E6;
}
</style>
