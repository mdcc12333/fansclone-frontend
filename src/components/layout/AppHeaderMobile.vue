<template>
  <header class="w-full bg-main py-2">
    <nav class="navbar navbar-expand-md navbar-light bg-light">
        <div class="order-1 order-md-0">
          <a class="navbar-brand mx-auto" href="#"><img src="../../assets/logo.svg" width="100px"></a>
          <!-- <button class="navbar-toggler" type="button" data-toggle="collapse" data-target=".dual-collapse2">
            <i class="bi-list" />
          </button> -->
        </div>
        <div class="navbar-nav order-1">
          <ul class="navbar-nav d-flex flex-row">
            <li class="nav-item mr-3">
              <b-button class="btn-primary" v-b-modal="'signin-modal'">Sign in</b-button>
            </li>
            <li class="nav-item mr-3">
              <b-button class="btn-primary">Sign up</b-button>
            </li>
            <a href="#" class="nav-link d-flex flex-column mr-3">
              <i class="bi-bell" />
            </a>
            <a v-b-toggle.sidebar-1 class="nav-link d-flex flex-column">
              <i class="bi-list" />
            </a>
          </ul>
        </div>
    </nav>
    <!-- <b-sidebar id="sidebar-1" title="Sidebar" shadow>
      <div class="px-3 py-2">
        <p>
          Cras mattis consectetur purus sit amet fermentum. Cras justo odio, dapibus ac facilisis
          in, egestas eget quam. Morbi leo risus, porta ac consectetur ac, vestibulum at eros.
        </p>
      </div>
    </b-sidebar> -->
    <b-modal id="signin-modal" size="md" hide-footer>
      <b-container fluid>
        <router-view name="login"></router-view>
      </b-container>
    </b-modal>
    <b-modal id="signup-modal" size="md" hide-footer>
      <b-container fluid>
        <router-view name="signup"></router-view>
      </b-container>
    </b-modal>
  </header>
</template>
<script>
export default{
  methods: {

  },
};

</script>
<style scoped>
  ul.navbar-nav i{
    text-align: center;
  }
  .btn-primary{
    background: #002D64;
    border-radius: 8px;
  }
</style>
