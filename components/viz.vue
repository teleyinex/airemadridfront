<template>
  <v-card
    class="mx-auto"
    dark
    max-width="600"
  >
    <v-card-title>
      <div class="cloud">
      <svg xmlns="http://www.w3.org/2000/svg" class="icon-clouds" viewBox="0 0 24 24">
        <path fill="none" stroke="#fff" d="M13.877 4.002a3 3 0 0 0-2.772 2.225 5.502 5.502 0 0 0-6.076 5.892A4 4 0 0 0 6 20h11.5a4.5 4.5 0 0 0 2.59-8.178A3.5 3.5 0 0 0 21 9.5a3.52 3.52 0 0 0-4.15-3.44 3 3 0 0 0-2.973-2.058z"/>
        <text x="7.569" y="15.589" stroke-width=".142" font-family="Roboto Condensed" font-size="4.685" font-weight="400" letter-spacing="0" style="line-height:1.25;-inkscape-font-specification:'Roboto Condensed, '" word-spacing="0">
        <tspan :x="formula !== 'PM10' && formula !== 'PM2.5' ? 9 : 8" y="15.589">{{formula}}</tspan>
        </text>
      </svg></div>
      <v-layout
        column
        align-start
      >
        <div class="caption grey--text text-uppercase">
          {{title.split('[')[0]}}
        </div>
        <div>
          <span
            class="display-2 font-weight-black"
            v-text="avg || '—'"
          ></span>
          <strong v-if="avg">{{title.split('[')[1].split(']')[0]}}</strong>
        </div>
      </v-layout>

      <v-spacer></v-spacer>

      <v-btn icon class="align-self-start" size="28">
        <v-icon>mdi-arrow-right-thick</v-icon>
      </v-btn>
    </v-card-title>

    <v-sheet color="transparent">
      <v-sparkline
        :key="String(avg)"
        :smooth="16"
        :gradient="gradient"
        :padding="padding"
        :line-width="3"
        :value="data"
        :labels="labels"
        :show-labels="true"
        auto-draw
        stroke-linecap="round"
      ></v-sparkline>
    </v-sheet>
  </v-card>
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

export default {
  props: {
    data: {
      default: [],
      type: Array
    },
    title: {
      default: '',
      type: String
    }
  },
  data() {
    return {
      gradient: gradients[2],
      labels: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24],
      gradientDirection: 'top',
      padding: 16,
      options: {
        'Mónoxido de Carbono[mg/m³]': 'CO',
        'Dióxido de Nitrógeno[µg/m³]': 'CO2',
        'Ozono[µg/m³]': 'O3',
        'Dióxido de Azufre[µg/m³]': 'SO2',
        'Dióxido de Nitrógeno[µg/m³]': 'NO2',
        'Partículas PM2.5[µg/m³]': 'PM2.5',
        'Partículas PM10[µg/m³]': 'PM10',
        'Benzeno[µg/m³]': 'C6H6'
      }
    }
  },
   computed: {
     formula() {
       return this.options[this.title]
     },
      avg () {
        const sum = this.data.reduce((acc, cur) => acc + cur, 0)
        const length = this.data.length

        if (!sum && !length) return 0

        return Math.ceil(sum / length)
      }
    }
}
</script>
<style lang="styl" scoped>
  .cloud
    width: 80px  
    svg
      text
        fill: white
        font-size: 4px
</style>
