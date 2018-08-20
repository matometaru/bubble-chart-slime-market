<template>
  <div id="app">
    <v-navigation-drawer fixed app>
      <v-container>
        <v-layout row wrap>
          <v-btn block depressed @click='addPoint'>Add Point</v-btn>
          <v-flex xs12>
          <v-text-field v-model='labels.x' label='X Axis Label' />
            </v-flex>
          <v-flex xs12>
          <v-text-field v-model='labels.y' label='Y Axis Label' />
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
          
          <v-flex xs12>
              <v-slider :label='`Radius ${radius}`' v-model='radius' min='1' max='10' step='1' thumb-label />
            </v-flex>
        </v-layout>
      </v-container>
    </v-navigation-drawer>
      
    <v-content>
      <v-container fluid fill-height>
        <v-layout justify-center align-center>
          <v-flex xs12>
            <v-chart :data='data' :labels='labels' :point-style='pointStyle' :radius='radius' />
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </div>
</template>

<script>
import Chart from '~/components/Chart.vue';

export default {
  components: {
    'v-chart': Chart,
  },
  data: () => ({
    data: [],
    pointStyle: {
      stroke: '#43e742',
      fill: '#ffffff',
      strokeWidth: 1,
    },
    radius: 4,
    drawer: true,
    labels: {
      x: 'X Axis',
      y: 'Y Axis',
    },
  }),
  created() {
    this.data = Array(10).fill(10).map((d, i) => {
      return {
        x: i,
        y: Math.random(),
      };
    });
  },
  methods: {
    addPoint() {
      this.data.push({
        x: this.data.length,
        y: Math.random(),
      })  ;
    },
  },
}
</script>

<style>
.test {
  background: #000;
}
</style>

