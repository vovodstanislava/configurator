<script>
export default {
  name: 'Products',
  data () {
    return {
      selectedComponent: 'motherboards',
      list: [
        { text: 'Motherboards', icon: 'mdi-chart-tree', value: 'motherboards' },
        { text: 'Processors', icon: 'mdi-memory', value: 'processors' },
        { text: 'Ram', icon: 'mdi-alpha-r-box', value: 'ram' },
        { text: 'Rom', icon: 'mdi-harddisk', value: 'rom' },
        { text: 'Videocards', icon: 'mdi-rectangle', value: 'videocards' },
        { text: 'Monitors', icon: 'mdi-monitor', value: 'monitors' },
        { text: 'Mouses', icon: 'mdi-rodent', value: 'mouses' },
        { text: 'Keyboards', icon: 'mdi-keyboard', value: 'keyboards' }
      ],
      items: {}
    }
  },
  computed: {
    cartComponents () {
      return this.$store.getters.getCart
    },
    filteredItems () {
      const items = Object.assign({}, this.items)
      const processorIndex = this.cartComponents.findIndex(item => item.type === 'processors')
      const motherboardIndex = this.cartComponents.findIndex(item => item.type === 'motherboards')
      const ramIndex = this.cartComponents.findIndex(item => item.type === 'ram')

      if (processorIndex !== -1) {
        const processor = this.cartComponents[processorIndex].product

        items.motherboards = items.motherboards.filter(item => item.socket === processor.socket)
      }

      if (motherboardIndex !== -1) {
        const motherboard = this.cartComponents[motherboardIndex].product

        items.processors = items.processors.filter(item => item.socket === motherboard.socket)
        items.ram = items.ram.filter(item => item.memoryType === motherboard.memoryType)
      }

      if (ramIndex !== -1) {
        const ram = this.cartComponents[ramIndex].product

        items.motherboards = items.motherboards.filter(item => item.memoryType === ram.memoryType)
      }

      return items
    }
  },
  async mounted () {
    await this.loadData()
  },
  methods: {
    async loadData () {
      try {
        this.items = (await import('../server/components')).default
      } catch {
        console.log('Load error')
      }
    },
    addToCart (product) {
      this.$store.dispatch('setProduct', {
        product,
        type: this.selectedComponent
      })
    }
  }
}
</script>

<template>
  <v-row>
    <v-col cols="12" sm="12" md="4" lg="3" xl="3">
      <v-card>
        <v-list>
          <v-subheader>Components</v-subheader>
          <v-list-item-group v-model="selectedComponent" color="deep-purple accent-4">
            <v-list-item
              v-for="(item, i) in list"
              :key="i"
              :value="item.value"
            >
              <v-list-item-icon>
                <v-icon v-text="item.icon"></v-icon>
              </v-list-item-icon>
              <v-list-item-content>
                <v-list-item-title v-text="item.text"></v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list-item-group>
        </v-list>
      </v-card>
    </v-col>
    <v-col cols="12" sm="12" md="8" lg="9" xl="9" class="pt-2 product-container">
      <v-row>
        <v-col :key="item.id" v-for="item in filteredItems[selectedComponent]" cols="12" sm="12" md="6" lg="4" xl="3" class="pa-1">
          <v-card height="435">
            <v-img
              :src="require('../' + item.image)"
              height="200px"
              :contain="true"
            ></v-img>

            <v-card-title>
              {{item.name}}
            </v-card-title>

            <v-card-subtitle>
              {{item.description}}
            </v-card-subtitle>

            <v-card-actions class="card-actions">
          <span class="title font-weight-light pl-2">
            {{item.price}} $
          </span>

              <v-spacer></v-spacer>
              <v-btn
                color="purple"
                class="pb-2 pr-4"
                icon
                @click="addToCart(item)"
              >
                <v-icon large>mdi-cart</v-icon>
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-col>
  </v-row>
</template>

<style scoped>
  .card-actions {
    position: absolute !important;
    bottom: 0;
    width: 100%;
  }

  .product-container {
    height: calc(100vh - 80px);
    overflow-x: auto;
  }

  .product-container::-webkit-scrollbar {
    width: 3px;
  }
  .product-container::-webkit-scrollbar-thumb {
    background: #f0f0f0;
  }
  .product-container::-webkit-scrollbar-thumb:hover {
    background: rgba(86, 52, 233, 0.9);
  }
</style>
