<template>
  <div id="app" class="container">
    <div class="row mt-5" v-if="arrPositive.length > 0">
      <div class="col">
        <h2>Positivos</h2>
        <line-chart :chartData="arrPositive" :options="chartOptions" label="Positivos"></line-chart>
      </div>
    </div>
  </div>
</template>

<script>
import moment from 'moment'
import axios from "axios";
import LineChart from './components/LineChart.vue';

export default {
  name: "App",
  components: {
    LineChart
  },
  data() {
    return { 
      arrPositive: [], 
      arrHospotalized: [],
      arrInIcu:[],
      arrOnVentilators:[],
      arrRecovered: [],
      arrDeaths:[],
      chartOptions:{
        reponsive:true,
        maintainAspectRatio:false
      }
    };
  },
  async created() {
    const { data } = await axios.get("http://covidtracking.com/api/us/daily");
    
    data.forEach(d=>{//varre o jason lendo os dados
      const date = moment(d.date, "YYYYMMDD").format("DD/MM");
      const {
        positive, 
        hospitalizedCurrently, 
        inIcuCurrently,
        onVentilatorsCurrently,
        recovered,
        death
      } = d;
      this.arrPositive.push({date, total:positive});
      this.arrHospotalized.push({date, total: hospitalizedCurrently});
      this.arrInIcu.push({date, total:inIcuCurrently});
      this.arrOnVentilators.push({date, total:onVentilatorsCurrently});
      this.arrRecovered.push({date, total:recovered});
      this.arrDeaths.push({date, total:death});

      console.log(this.arrPositive);
    })
  },
};
</script>

<style>
</style>
