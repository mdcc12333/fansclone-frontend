<template>
  <b-link :to="user.url" class="rounded suggestion w-100 bg-light d-block">
    <b-img :src="user.cover" v-if="user.cover != null" class="rounded cover" />
    <b-avatar
      :src="user.avatar"
      :text="user.initials"
      size="68px"
      class="avatar m-2"
    />
    <div class="image-text">
      <div class="myalign">
        {{ user.name }}
        <b-dropdown no-caret right variant="link">
          <template slot="button-content"
          ><i class="bi-three-dots-vertical text-white"
          /></template>
          <b-dropdown-item @click.prevent="copyLink">{{
              $t("general.copy-link-to-profile")
            }}</b-dropdown-item>
          <b-dropdown-item @click.prevent="addToList(user)">{{
              $t("general.add-to-list")
            }}</b-dropdown-item>
        </b-dropdown>
      </div>
    </div>
<!--    <div class="buttons m-2 pt-4">-->
<!--      -->
<!--    </div>-->
<!--    <div class="overflow-hidden w-100 subprofile py-2 pr-2">-->
<!--      <ui-username :user="user" :asLink="false" class="text-white" />-->
<!--      <div class="text-white small username d-block">-->
<!--        {{ "@" + user.username }}-->
<!--      </div>-->
<!--    </div>-->
  </b-link>
</template>
<style lang="scss" scoped>
.suggestion {
  position: relative;
  height: 100px;
  .avatar {
    position: relative;
    z-index: 1;
    margin: 14px!important;
  }
  .cover {
    -o-object-fit: cover;
    object-fit: cover;
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
  }
  .subprofile {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    z-index: 0;
    padding-left: calc(100px + 1rem);
    border-bottom-left-radius: 0.25rem !important;
    border-bottom-right-radius: 0.25rem !important;
  }
  .image-text {
    width: 200px;
    height: 100%;
    position: absolute;
    top: 0;
    right: 0;
    z-index: 11;
    display: flex;
    justify-content: center;
    align-content: center;
    flex-direction: column;
    font-weight: 500;
    font-size: 14px;
    color: #fff;
    button {
      padding: 0;
    }
  }
  .b-dropdown{
    float:right;
    margin-right:15px;
    margin-left: auto;
  }
  .myalign{
    display: flex;
    justify-content: center;
    align-items: center;
  }
}
.b-avatar:before{
  content: "";
  position: absolute;
  top: -4px;
  left: -4px;
  right: -4px;
  bottom: -4px;
  border-radius: 50%;
  border: 2px solid transparent;
  background: linear-gradient(0deg,#F7A34B,#DE0046) border-box;
  -webkit-mask:
      linear-gradient(#fff 0 0) padding-box,
      linear-gradient(#fff 0 0);
  -webkit-mask-composite: destination-out;
  mask-composite: exclude;
}
</style>
<script>
import User from "../models/User";
export default {
  props: {
    user: User,
  },
  methods: {
    copyLink() {
      const link = `${process.env.VUE_APP_APP_URL}${this.user.url}`;
      this.$copyText(link);
    },
    addToList(user) {
      this.$store.state.addToListUser = user;
    },
  },
};
</script>
