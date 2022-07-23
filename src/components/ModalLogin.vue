<template>
  <div>
    <div class="title">Let’s Sign You In</div>
    <div class="subtitle">Fill out info below to sign in!</div>
    <div v-if="errors._ && errors._.length > 0">
      <div class="alert alert-danger" v-for="error in errors._" :key="error">
        {{ error }}
      </div>
    </div>

    <br><br>
    <v-text-field
        v-model="email"
        :label="$t('general.email')"
        name="email"
        outlined
        :error-messages="errors.email"
        class="email-input"
    ></v-text-field>
    <v-text-field
        v-model="password"
        :label="$t('general.password')"
        outlined
        type="password"
        name="password"
        :error-messages="errors.password"
        class="password-input"
    ></v-text-field>
    <div class="d-flex small align-items-center justify-content-center mb-3">
      <b-link class="forgot-password-text" to="/forgot">{{ $t("general.forgot-password") }}</b-link>
    </div>

    <b-button
      variant="primary"
      style="padding: 11px;"
      class="w-100 mb-3 text-white"
      @click.prevent="submitForm"
      >{{ $t("general.login") }}</b-button
    >

    <div class="d-flex small align-items-center justify-content-center">
      <span class="sub-signup-text mr-2">Don’t have an account?  </span><b-link class="sign-up-text" to="/signup">{{ $t("general.signup") }}</b-link>
    </div>

    <!-- <GoogleLogin
      :params="google.params"
      :onSuccess="googleSuccess"
      :onFailure="failure"
      class="mt-5 w-100 btn btn-google"
    >
      <div class="icon">
        <b-img :src="google.icon" />
      </div>
      {{ $t("general.sign-in-with-google") }}
    </GoogleLogin> -->
  </div>
</template>
<style scoped lang="scss">
@import "~@/assets/scss/_variables.scss";
.btn-google {
  background-color: #4285f4;
  color: #ffffff;
  position: relative;
  .icon {
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 2.5rem;
    background: #ffffff;
    display: flex;
    border-top-left-radius: $border-radius;
    border-bottom-left-radius: $border-radius;
    img {
      display: block;
      margin: auto;
      height: 60%;
    }
  }
}
</style>
<script>
// import GoogleLogin from "vue-google-login";
import User from "./models/User";
export default {
  name: 'ModalLogin',
  data() {
    return {
      email: "",
      password: "",
      errors: {
        email: [],
        password: []
      },
    };
  },
  computed: {
    google() {
      return {
        params: {
          client_id: process.env.VUE_APP_GOOGLE_CLIENT_ID,
          scope: "profile email",
        },
        renderParams: {
          longtitle: true,
          height: 36,
          theme: "dark",
        },
        icon: require("@/assets/google.svg"),
      };
    },
  },
  methods: {
    submitForm() {
      this.login(User.CHANNEL_EMAIL);
    },
    googleSuccess(googleUser) {
      this.login(
        User.CHANNEL_GOOGLE,
        googleUser.getAuthResponse().access_token
      );
    },
    login(type, token) {
      this.errors = {
        email: [],
        password: []
      };
      let fields = {
        channel_type: type,
      };
      if (type == User.CHANNEL_EMAIL) {
        fields.email = this.email;
        fields.password = this.password;
      } else {
        fields.token = token;
      }
      this.$post(
        "/auth/login",
        fields,
        (data) => {
          this.$saveToken(data.token);
          this.$saveUser(data.user);
          this.$emit('close')
        },
        (errors) => {
          this.errors = errors;
        }
      );
    },
    failure() {
      this.$bvToast.toast(this.$t("general.login-failed"), {
        autoHideDelay: 2000,
        title: this.$t("general.error"),
        solid: true,
        toaster: "b-toaster-bottom-left",
      });
    },
  },
};
</script>
