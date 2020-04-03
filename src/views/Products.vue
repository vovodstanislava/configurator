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
  async mounted () {
    await this.loadData()
  },
  methods: {
    async loadData () {
      try {
        this.items = (await import('../server/components')).default
        console.log(this.items)
      } catch {
        console.log('Load error')
      }
    }
  }
}
</script>

<template>
  <v-row>
    <v-col cols="3">
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
    <v-col cols="9" class="pt-2">
      <v-row>
        <v-col :key="item.id" v-for="item in items[selectedComponent]" cols="3" class="pa-1">
          <v-card height="435">
            <v-img
              :src="require('../'+item.image)"
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
.card-actions{
  position: absolute !important;
  bottom: 0;
  width: 100%;
}
</style>
