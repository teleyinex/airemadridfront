<template>
  <v-layout
    column
   >
    <v-flex
      xs12
      >
      <v-dialog
        v-model="loading"
        hide-overlay
        persistent
        width="300"
        >
        <v-card
        color="primary"
        dark
        >
        <v-card-text>
          <v-progress-linear
        indeterminate
        color="white"
        class="mb-0"
        ></v-progress-linear>
        </v-card-text>
        </v-card>
      </v-dialog>
      <v-card class="mx-auto" dark max-width="600">
        <v-card-title>
          <div>
          <h1 style="color: white;">Escoge una estación y una fecha para ver la contaminación</h1>
          <v-select
             label="Estación"
             v-model="station"
             :items="stations"
             @input="getData"
             item-text="name"
             item-value="id"
             full-width
             dark
             ></v-select>
          <v-menu
           v-model="menu2"
           :close-on-content-click="false"
           lazy
           transition="scale-transition"
           offset-y
           full-width
           min-width="290px"
           >
           <v-text-field
           slot="activator"
           v-model="date"
           prepend-icon="event"
           readonly
           ></v-text-field>
             <v-date-picker v-model="date" :max="maxDate.toISOString()" @input="updateDate"></v-date-picker>
          </v-menu>
          </div>
        </v-card-title>


      </v-card>
      <div class="mx-auto" style="padding: 16px;">
      </div>
      <viz v-if="!loading" v-for="k in keys" :key="k" :data="aire[k]" :title="k" style="margin-bottom: 50px;"/>
    </v-flex>
  </v-layout>
</template>

<script>
const gradients = [
   ['#222'],
   ['#42b3f4'],
   ['red', 'orange', 'yellow'],
   ['purple', 'violet'],
   ['#00c6ff', '#F0F', '#FF0'],
   ['#f72047', '#ffd200', '#1feaea']
 ]
import stations from '~/static/stations.json'
import {orderBy} from 'lodash'
import viz from '~/components/viz.vue'
export default {
  components: {
    viz
  },
  async asyncData({app}) {
    let maxDate = new Date()
    maxDate = new Date(maxDate.setDate(maxDate.getDate() - 1))
    let st = stations
    st = orderBy(st, ['name'])
    return { aire: {}, stations: st, maxDate, date: maxDate.toISOString().substr(0, 10) }
  },
  data: () => ({
    loading: false,
    menu2: false,
    width: 2,
    radius: 10,
    padding: 16,
    lineCap: 'round',
    gradient: gradients[2],
    value: [0, 2, 5, 9, 5, 10, 3, 5, 0, 0, 1, 8, 2, 9, 0],
    gradientDirection: 'top',
    labels: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24],
    gradients,
    station: null,
  }),
  computed: {
    computedDateFormatted () {
       return this.formatDate(this.date)
    },
    keys() {
      return Object.keys(this.aire).filter(k => k.includes('['))
    }
  },
  methods: {
    formatDate (date) {
        if (!date) return null
        const [year, month, day] = date.split('-')
        return `${day}/${month}/${year}`
    },
    async getData() {
      this.loading = true
      const url = `https://aireapi.daniellombrana.es/?estacion=${this.station}&date=${this.formatDate(this.date)}`
      this.aire = await this.$axios.$get(url)
      this.loading = false 
    },
    updateDate() {
      this.menu2 = false
      this.getData()
    }
  }
}
</script>
<style lang="styl">
text 
  fill: white
  font-size: 5px
</style>
