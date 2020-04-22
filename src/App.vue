<script>
import Cart from '@/components/Cart'

export default {
  name: 'App',
  components: {
    Cart
  },
  data: () => ({
    snackbar: false
  }),
  methods: {
    showSnack () {
      this.snackbar = true
    }
  }
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
        <v-app-bar-nav-icon to="/">
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

        <Cart :showSnack = "showSnack"/>
      </v-app-bar>

      <v-content class="router-view">
          <router-view></router-view>
        <v-snackbar
          v-model="snackbar"
          color="green"
          bottom
          right
        >
          Order is processed!
          <v-btn
            text
            @click="snackbar = false"
          >
            Close
          </v-btn>
        </v-snackbar>
      </v-content>
    </v-app>
  </div>
</template>

<style>
  .wrapper, .router-view {
    padding-left: 11%!important;
    padding-right: 11%!important;
  }
  html {
    overflow: hidden !important;
  }
</style>
