<template>
  <layout-authorized v-if="isLoggedIn" />
  <layout-discovery v-else />
</template>
<script>
import LayoutAuthorized from "./LayoutAuthorized.vue";
import LayoutDiscovery from "./LayoutDiscovery.vue";
export default {
  computed: {
    isLoggedIn: function () {
      return this.$store.state.token != null;
    },
  },
  components: { LayoutAuthorized, LayoutDiscovery },
  mounted() {
    if (this.isLoggedIn) {
      this.$get(
        "/auth/me",
        (data) => {
          this.$saveUser(data);
        },
        () => {}
      );
    }
  },
};
</script>
