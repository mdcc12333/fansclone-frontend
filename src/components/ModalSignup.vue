<template>
  <b-form @submit.prevent="submitForm">
    <p class="title">Getting Started</p>
    <p class="subtitle">Create an account to continue!</p>
    <div v-if="errors._ && errors._.length > 0">
      <div class="alert alert-danger" v-for="error in errors._" :key="error">
        {{ error }}
      </div>
    </div>

    <ui-form-input
      type="text"
      name="name"
      v-model="name"
      :errors="errors"
      :label="$t('general.your-name')"
    />

    <ui-form-input
      type="text"
      name="email"
      v-model="email"
      :errors="errors"
      :label="$t('general.your-email')"
    />

    <ui-form-input
      type="password"
      name="password"
      v-model="password"
      :errors="errors"
      :label="$t('general.password')"
    />

    <!-- <i18n path="general.signup-agree" tag="div" class="small text-center mb-3">
      <template v-slot:terms>
        <b-link :href="url + '/terms.html'" target="_blank">{{
          $t("general.terms-of-use")
        }}</b-link>
      </template>
      <template v-slot:privacy>
        <b-link :href="url + '/privacy.html'" target="_blank">{{
          $t("general.privacy-policy")
        }}</b-link>
      </template>
    </i18n> -->

    <b-button type="submit" variant="primary" class="w-100 mb-3">{{
      $t("general.create-account")
    }}</b-button>
    <div class="d-flex small align-items-center justify-content-center">
      <span class="sub-signup-text mr-2">Already have an account?  </span><b-link class="sign-up-text" to="/signin">{{ $t("general.login") }}</b-link>
    </div>
  </b-form>
</template>
<script>
import UiFormInput from "./ui/UiFormInput.vue";
export default {
  name: 'ModalSignup',
  components: { UiFormInput },
  data() {
    return {
      email: "",
      password: "",
      name: "",
      errors: {},
    };
  },
  computed: {
    url() {
      return process.env.VUE_APP_APP_URL;
    },
  },
  methods: {
    submitForm() {
      this.errors = {};
      this.$post(
        "/auth/signup",
        {
          email: this.email,
          password: this.password,
          name: this.name,
        },
        (data) => {
          this.$saveToken(data.token);
          this.$saveUser(data.user);
          location = process.env.VUE_APP_APP_URL;
        },
        (errors) => {
          this.errors = errors;
        }
      );
    },
  },
};
</script>
