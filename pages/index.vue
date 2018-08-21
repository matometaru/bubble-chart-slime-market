<template>
  <div id="app">
    <v-navigation-drawer fixed app v-model="drawer">
      <v-container>
        <v-layout row wrap>
          <v-btn block depressed @click='addPoint'>Add Point</v-btn>
          
          <v-flex xs12>
            <v-text-field v-model='number' label='証券コード' />
          </v-flex>

          <v-flex xs12>
              <label>Fill Color ({{pointStyle.fill}}):
                <input type='color' v-model='pointStyle.fill'>
              </label>
            </v-flex>
          <v-flex xs12>
              <label>Stroke Color ({{pointStyle.stroke}}):
                <input type='color' v-model='pointStyle.stroke'>
              </label>
            </v-flex>
          <v-flex xs12>
              <v-text-field type='number' label='point stroke width' v-model.number='pointStyle.strokeWidth' />
            </v-flex>
        </v-layout>
      </v-container>
    </v-navigation-drawer>
      
    <v-content>
      <v-container fluid fill-height>
        <v-layout justify-center align-center>
          <v-flex xs12>
            <v-chart :data='data' :labels='labels' :point-style='pointStyle' />
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </div>
</template>

<script>
import Vue from 'vue'
import Chart from '~/components/Chart.vue';

export default {
  components: {
    'v-chart': Chart,
  },
  data: () => ({
    id: 0,
    data: [],
    pointStyle: {
      stroke: '#43e742',
      fill: '#ffffff',
      strokeWidth: 1,
    },
    number: 3798, // 銘柄番号
    value: 13067, // 時価総額
    name: 'ULSグループ',
    drawer: true,
    labels: {
      x: '年月日',
      y: 'PER',
    },
  }),
  created() {
    this.data = Array(10).fill(10).map((d, i) => {
      return {
        id: this.id++,
        x: i,
        y: Math.random(),
        radius: Math.random() * 100,
        number: 3798,
        value: this.value,
        name: this.name,
        style: {
          stroke: '#ddd',
          fill: '#fff',
          strokeWidth: 1,
        }
      };
    });
  },
  methods: {
    addPoint() {
      this.data.push({
        id: this.id++,
        x: this.data.length,
        y: Math.random(),
        radius: 4,
        number: this.number,
        value: this.value,
        name: this.name,
        style: Vue.util.extend({}, this.pointStyle)
      });
    },
  },
}
</script>

<style>
.test {
  background: #000;
}
</style>

