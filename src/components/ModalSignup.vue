<template>
  <b-form @submit.prevent="submitForm">
    <p class="title">Getting Started</p>
    <p class="subtitle">Create an account to continue!</p>
    <div v-if="errors._ && errors._.length > 0">
      <div class="alert alert-danger" v-for="error in errors._" :key="error">
        {{ error }}
      </div>
    </div>

    <div class="avatar position-relative rounded-circle">
      <b-avatar text=" " size="100px" style="cursor: pointer">
        <div class="buttons d-flex w-full h-full" v-on:click="clck('avatar')">
          <img width="100" :src="avatarPlaceholder" alt="avatar"/>
        </div>
      </b-avatar>
    </div>
    <p class="subtitle mb-4">Upload Profile Image</p>

    <v-text-field
        v-model="name"
        :label="$t('general.your-name')"
        name="name"
        outlined
        :error-messages="errors.name"
        class="name-input"
    ></v-text-field>
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

    <vue-tel-input-vuetify
        v-model="phone"
        :label="$t('general.phone-number')"
        outlined
        type="number"
        name="phone"
        :error-messages="errors.phone"
        class="phone-input"
        v-on:country-changed="countryChanged"
    ></vue-tel-input-vuetify>

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

    <b-button type="submit"
              variant="primary"
              style="padding: 11px;"
              class="w-100 mb-3 text-white">
      {{ $t("general.create-account") }}
    </b-button>
    <div class="d-flex small align-items-center justify-content-center">
      <span class="sub-signup-text mr-2">Already have an account?  </span>
      <b-link class="sign-up-text" to="/signin">{{ $t("general.login") }}</b-link>
    </div>

    <input type="file" ref="avatar" hidden name="avatar"/>
  </b-form>
</template>
<script>
import VueTelInputVuetify from "vue-tel-input-vuetify/lib/vue-tel-input-vuetify.vue"

export default {
  name: 'ModalSignup',
  data() {
    return {
      email: "",
      password: "",
      name: "",
      phone: "",
      errors: {
        email: [],
        password: [],
        phone: [],
        name: []
      },
      country: null,
      avatar: null,
    };
  },
  components: {
    VueTelInputVuetify,
  },
  computed: {
    url() {
      return process.env.VUE_APP_APP_URL;
    },
    avatarPlaceholder() {
      return process.env.VUE_APP_API_URL + '/storage/profile/avatar/avatar.png'
    }
  },
  methods: {
    clck(type) {
      console.log(12, type)
      this.$refs[type].click();
    },
    countryChanged(country){
      this.country = '+' + country.dialCode
    },
    submitForm() {
      this.errors = {
        email: [],
        password: [],
        phone: [],
        name: []
      }
      let formData = new FormData();
      formData.append("image", this.$refs['avatar'].files[0]);
      formData.append("email", this.email);
      formData.append("password", this.password);
      formData.append("name", this.name);
      formData.append("phone", this.phone);
      formData.append("email", this.email);
      this.$post(
          "/auth/signup", formData,
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
  },
};
</script>
<style scoped>
.avatar {
  margin-top: 40px;
  justify-content: center;
  display: flex;
  margin-bottom: 15px;
}

</style>