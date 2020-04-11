<script>
export default {
  name: 'Cart',
  data () {
    return {
      dialog: false,
      valid: true,
      user: {
        firstName: '',
        secondName: '',
        phone: '',
        email: '',
        comment: '',
        promo: ''
      },
      nameRules: [
        v => !!v || 'Field is required',
        v => (v && v.length > 2) || 'Field must be less than 2 characters'
      ],
      emailRules: [
        v => !!v || 'E-mail is required',
        v => /.+@.+\..+/.test(v) || 'E-mail must be valid'
      ],
      phoneRules: [
        v => !!v || 'Phone is required',
        v => (v && v.length === 9) || 'Phone must be valid'
      ]
    }
  },
  computed: {
    items () {
      return this.$store.getters.getCart
    },
    totalPrice () {
      return this.items.reduce((sum, item) => (item.quantity * item.product.price) + sum, 0)
    },
    promotion () {
      if (this.user.promo === '1111') {
        return 500
      }
      return 0
    }
  },
  methods: {
    validate () {
      this.$refs.form.validate()
    },
    reset () {
      this.$refs.form.reset()
    },
    checkout () {
      this.validate()
      this.$nextTick(() => {
        if (this.valid) {
          this.saveToLocalstorage()
          this.clearCart()
          this.reset()
          this.dialog = false
        }
      })
    },
    deleteItem (index) {
      this.$store.dispatch('deleteProduct', index)
    },
    saveToLocalstorage () {
      const orders = JSON.parse(localStorage.getItem('orders')) || []
      const order = {
        items: this.items,
        price: this.totalPrice,
        pay: this.totalPrice - this.promotion,
        user: Object.assign({}, this.user)
      }

      orders.push(order)

      this.$store.dispatch('setOrder', order)

      localStorage.setItem('orders', JSON.stringify(orders))
    },
    clearCart () {
      this.$store.dispatch('clearCart')
    }
  }
}
</script>

<template>
<div>
  <v-menu
    left
    bottom
    :close-on-content-click="false"
    offset-y
  >
    <template v-slot:activator="{ on }">
      <v-btn icon v-on="on">
        <v-badge
          color="red"
          :content="items.length"
          :value="items.length"
          overlap
        >
          <v-icon large>mdi-cart</v-icon>
        </v-badge>
      </v-btn>
    </template>
    <v-card v-if="items.length">
      <v-list subheader>
        <v-subheader>Cart</v-subheader>

        <v-list-item
          v-for="(item, index) in items"
          :key="index"
          @click="()=>{}"
        >
          <v-list-item-avatar>
            <v-img
              :src="require('../' + item.product.image)"
              :contain="true"
            ></v-img>
          </v-list-item-avatar>

          <v-list-item-content>
            <v-list-item-title v-text="item.product.name"></v-list-item-title>
          </v-list-item-content>

          <v-list-item-action class="ml-7">
            <div class="cart-price">
              <div class="text-input">
                <v-text-field v-model="item.quantity" hide-details type="number"></v-text-field>
              </div>
              <div> x {{item.product.price}} $</div>
              <div>
                <v-btn class="ml-3" icon @click="deleteItem(index)">
                  <v-icon>mdi-close-circle</v-icon>
                </v-btn>
              </div>
            </div>
          </v-list-item-action>
        </v-list-item>
      </v-list>
      <v-divider></v-divider>
      <v-card-text class="d-flex">
        <span class="subtitle-1 font-regular mt-1"> Total: {{totalPrice}} $</span>
        <v-spacer></v-spacer>
        <v-btn color="deep-purple accent-4" class="text-capitalize" @click="dialog = true" dark>checkout</v-btn>
      </v-card-text>
    </v-card>
    <v-card v-else>
      <v-card-text>
        Card is empty!
      </v-card-text>
    </v-card>
  </v-menu>
  <v-dialog v-model="dialog" persistent max-width="600px">
    <v-card>
      <v-card-title>
        <span class="headline">Checkout</span>
      </v-card-title>
      <v-card-text>
        <v-container>
          <v-form
            ref="form"
            v-model="valid"
          >
          <v-row>
            <v-col cols="6" class="pa-1">
              <v-text-field label="First name*" :rules="nameRules" v-model="user.firstName" required></v-text-field>
            </v-col>
            <v-col cols="6" class="pa-1">
              <v-text-field label="Second name*" :rules="nameRules" v-model="user.secondName" required></v-text-field>
            </v-col>
            <v-col cols="6" class="pa-1">
              <v-text-field
                label="Phone*"
                persistent-hint
                :rules="phoneRules"
                required
                v-model="user.phone"
              ></v-text-field>
            </v-col>
            <v-col cols="6" class="pa-1">
              <v-text-field label="Email*" :rules="emailRules" v-model="user.email" required></v-text-field>
            </v-col>
            <v-col cols="12" class="pa-1">
              <v-textarea label="Comment" v-model="user.comment"></v-textarea>
            </v-col>
            <v-col cols="12" class="pa-1">
              <v-text-field label="Promotional code" v-model="user.promo"></v-text-field>
            </v-col>
          </v-row>
          </v-form>
        </v-container>

        <v-row>
          <v-col cols="12" class="subtitle-1 py-0 text-right font-weight-black">Total: {{totalPrice}} $</v-col>
          <v-col cols="12" class="subtitle-1 py-0 text-right font-weight-black">Promotion: {{promotion}} $</v-col>
          <v-col cols="12" class="subtitle-1 py-0 text-right font-weight-black">Pay: {{totalPrice - promotion}} $</v-col>
        </v-row>
      </v-card-text>
      <v-card-actions>
        <small class="caption ml-3">*indicates required field</small>
        <v-spacer></v-spacer>
        <v-btn color="deep-purple accent-4" text @click="dialog = false">Close</v-btn>
        <v-btn color="deep-purple accent-4" text @click="checkout()">Checkout</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</div>
</template>

<style scoped>
  .text-input {
    width: 40px;
  }

  .cart-price {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
  }

  .cart-price > div {
    align-self: center;
  }

  .cart-price > div:last-child {
    justify-self: end;
  }
</style>
