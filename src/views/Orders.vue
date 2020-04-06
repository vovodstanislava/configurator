<script>
export default {
  name: 'Orders',
  computed: {
    orders () {
      return this.$store.getters.getOrders
    }
  }
}
</script>

<template>
  <v-row>
    <pre>{{orders}}</pre>
  </v-row>
</template>

<style scoped>

</style>
