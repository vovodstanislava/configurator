<script>
import Cart from '@/components/Cart'

export default {
  name: 'App',
  components: {
    Cart
  },
  data: () => ({
    //
  })
}
</script>

<template>
  <div id="app">
    <v-app>
      <v-app-bar
        app
        color="deep-purple accent-4"
        dark
        height="60"
        class="wrapper"
      >
        <v-app-bar-nav-icon>
          <v-icon large>
            mdi-laptop
          </v-icon>
        </v-app-bar-nav-icon>

        <span class="title ml-3 mr-5">Configurator <span class="font-weight-light">PC</span></span>

        <v-spacer></v-spacer>

        <v-btn text to="/">
          Products
        </v-btn>

        <v-btn text class="mx-3" to="/orders">
          Orders
        </v-btn>

        <Cart />
      </v-app-bar>

      <v-content class="router-view">
          <router-view></router-view>
      </v-content>
    </v-app>
  </div>
</template>

<style scoped>
  .wrapper, .router-view {
    padding-left: 15%!important;
    padding-right: 15%!important;
  }

</style>
