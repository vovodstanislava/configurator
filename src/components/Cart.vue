<script>
export default {
  name: 'Cart'
}
</script>

<template>
  <v-menu
    left
    bottom
    offset-y
  >
    <template v-slot:activator="{ on }">
      <v-btn icon v-on="on">
        <v-badge
          color="green"
          :content="0"
          :value="0"
          overlap
        >
          <v-icon large>mdi-cart</v-icon>
        </v-badge>
      </v-btn>
    </template>

    <v-list>
      <v-list-item
        v-for="n in 5"
        :key="n"
        @click="() => {}"
      >
        <v-list-item-title>Option {{ n }}</v-list-item-title>
      </v-list-item>
    </v-list>
  </v-menu>
</template>

<style scoped>

</style>
