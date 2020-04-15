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
    <v-simple-table dense class="order-table">
      <thead>
      <tr>
        <th>Name</th>
        <th>Date</th>
        <th>Contacts</th>
        <th>Comment</th>
        <th>Products</th>
        <th>Promo code</th>
        <th>Price</th>
        <th>Total price</th>
      </tr>
      </thead>
      <tbody>
      <tr :key="index" v-for="(order, index) in orders">
        <td>{{order.user.firstName}} {{order.user.secondName}}</td>
        <td>{{order.date}}</td>
        <td>
          <div>{{order.user.phone}}</div>
          <div>{{order.user.email}}</div>
        </td>
        <td>
          {{order.user.comment}}
        </td>
        <td>
          <v-list dense>
            <v-list-item
              dense
              v-for="(item, index) in order.items"
              :key="index"
            >
              <v-list-item-avatar class="ma-0">
                <v-img
                  :src="require('../' + item.product.image)"
                  :contain="true"
                  height="15"
                ></v-img>
              </v-list-item-avatar>

              <v-list-item-content class="pa-0">
                <v-list-item-title v-text="item.product.name"></v-list-item-title>
              </v-list-item-content>

              <v-list-item-action class="ma-0">
                 {{item.quantity}} x {{item.product.price}} $
              </v-list-item-action>
            </v-list-item>
          </v-list>
        </td>
        <td>
          {{order.user.promo}}
        </td>
        <td>
          {{order.price}} $
        </td>
        <td>
          {{order.pay}} $
        </td>
      </tr>
      </tbody>
    </v-simple-table>
  </v-row>
</template>

<style scoped>
.order-table {
  width: 100%;
  margin-top: 16px;
}
</style>
