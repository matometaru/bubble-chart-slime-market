<template>
  <div id="app">
    <v-navigation-drawer fixed app v-model="drawer">
      <v-container>
        <v-layout row wrap>
          <v-flex xs12>
            <v-btn block depressed @click='getStock'>Get Stock</v-btn>
          </v-flex>

          <v-flex xs12>
            <v-btn block depressed @click='addPoint'>Add Point</v-btn>
          </v-flex>
          
          <v-flex xs12>
            <v-text-field v-model='code' label='証券コード' />
          </v-flex>

          <v-flex xs12>
            <label>Fill Color ({{pointStyle.fill}}):
              <input type='color' v-model='pointStyle.fill'>
            </label>
          </v-flex>
        </v-layout>
      </v-container>
    </v-navigation-drawer>
      
    <v-content>
      <v-container fluid fill-height>
        <v-layout row wrap justify-center>

          <v-flex
            xs10
            mt-5
          >
            <v-slider
              min="0"
              :max="seasons.length-1"
              v-model="slider"
              thumb-label="always"
              thumb-size="64"
            >
              <template
                slot="thumb-label"
                slot-scope="props"
              >  
                <span>{{ season(props.value) }}</span>
              </template>
            </v-slider>
          </v-flex>

          <v-flex xs12>
            <v-chart :data='data' :labels='labels' />
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </div>
</template>

<script>
import Vue from 'vue'
import axios from 'axios'
import Chart from '~/components/Chart.vue';

export default {
  components: {
    'v-chart': Chart,
  },
  data: () => ({
    id: 0,
    all: [],
    data: [],
    pointStyle: {
      stroke: '#dddddd',
      fill: '#ffffff',
      strokeWidth: 1,
    },
    code: 1301, // 銘柄番号
    value: 13067, // 時価総額
    name: 'ULSグループ',
    drawer: true,
    labels: {
      x: 'ROE',
      y: 'PER',
    },
    seasons: [
      '2017-03',
      '2017-06',
      '2017-09',
      '2017-12',
      '2018-03',
      '2018-06'
    ]
  }),
  created() {
    // this.codeから各種情報を取得
    let stock = () => {
      return new Promise((resolve, reject) => {
        axios.get('http://localhost:3000/stocks/stocklist.json')
          .then((res) => {
            if (res.status !== 200) {
              reject();
            }
            this.all = res.data;
            resolve();
          })
      });
    }
    let display = async () => {
      try {
        await stock()
      } catch(err) {
        console.log("エラーが起こりました:" + err)
      }
    }
    display()
  },
  methods: {
    addPoint() {
      this.data.push({
        id: this.id++,
        x: this.pbr,
        y: this.per,
        radius: this.value/10,
        code: this.code,
        name: this.name,
        style: Vue.util.extend({}, this.pointStyle)
      });
    },
    getStock() {
      const filterNumber = this.code;
      let data = this.all;
      if (filterNumber) {
        data = data.filter((row)=> {
          return filterNumber == row.code
        })
      }
      data = data[0];
      this.per = data.date['2018-08-21'].per;
      this.pbr = data.date['2018-08-21'].pbr;
      this.value = data.date['2018-08-21'].volume;
      this.name = data.name;
      this.pointStyle.fill = data.color;
    },
    season (val) {
      return this.seasons[val]
    }
  },
}
</script>

<style>
</style>

