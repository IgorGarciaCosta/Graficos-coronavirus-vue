<template>
  <div id="app" class="container">
    <div class="row mt-5">
      <div class="col text-center">
        <h1>Visualização gráfica de dados do COVID-19</h1>
      </div>
    </div>

    <div class="row mt-5" v-if="arrPositive.length > 0">
      <div class="col">

        <div class="grafico">
          <h2>Positivos</h2>
          <line-chart :chartData="arrPositive" :options="chartOptions" label="Positivos" :chartColors="positiveChartColors"></line-chart>
          
        </div>
        
        <div class="grafico">
          <h2>Hospitalizados</h2>
          <line-chart :chartData="arrHospotalized" :options="chartOptions" label="Hospitalizados" :chartColors="hospitalizedChartColors"></line-chart>
        </div> 

        <div class="grafico">
          <h2>UTI</h2>
          <line-chart :chartData="arrInIcu" :options="chartOptions" label="UTI" :chartColors="inIcuChartColors"></line-chart>
        </div> 

        <div class="grafico">
          <h2>Em ventiladores</h2>
          <line-chart :chartData="arrOnVentilators" :options="chartOptions" label="Em ventiladores" :chartColors="onVentilatorsChartColors"></line-chart>
        </div> 

        <div class="grafico">
          <h2>Mortos</h2>
          <line-chart :chartData="arrDeaths" :options="chartOptions" label="Mortos" :chartColors="deathsChartColors"></line-chart>
        </div>

        <div class="grafico">
          <h2>Recuperados</h2>
          <line-chart :chartData="arrRecovered" :options="chartOptions" label="Recuperados" :chartColors="recoveredChartColors"></line-chart>
        </div>  

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
      positiveChartColors:{
        borderColor: "#077187",
        pointBorderColor:"#0e1428",
        pointBackgroundCOlor:"afd6ac",
        backgroundColor:"#74a57f"
      },
      arrHospotalized: [],
      hospitalizedChartColors:{
        borderColor: "#251f47",
        pointBorderColor:"#260f26",
        pointBackgroundCOlor:"858eab",
        backgroundColor:"#858eab"
      },
      arrInIcu:[],
      inIcuChartColors:{
        borderColor: "#190b28",
        pointBorderColor:"#190b28",
        pointBackgroundCOlor:"e55381",
        backgroundColor:"#e55381"
      },
      arrOnVentilators:[],
      onVentilatorsChartColors:{
        borderColor: "#784f41",
        pointBorderColor:"#784f41",
        pointBackgroundCOlor:"bbe5ed",
        backgroundColor:"#bbe5ed"
      },
      arrRecovered: [],
      recoveredChartColors:{
        borderColor: "#4e5e66",
        pointBorderColor:"#4e5e66",
        pointBackgroundCOlor:"31e981",
        backgroundColor:"#31e981"
      },
      arrDeaths:[],
      deathsChartColors:{
        borderColor: "#e06d06",
        pointBorderColor:"#e06d06",
        pointBackgroundCOlor:"402a2c",
        backgroundColor:"#402a2c"
      },
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
        onVentilatorCurrently,
        recovered,
        death
      } = d;
      this.arrPositive.push({date, total:positive});
      this.arrHospotalized.push({date, total: hospitalizedCurrently});
      this.arrInIcu.push({date, total:inIcuCurrently});
      this.arrOnVentilators.push({date, total:onVentilatorCurrently});
      this.arrRecovered.push({date, total:recovered});
      this.arrDeaths.push({date, total:death});

      console.log(this.arrPositive);
    })
  },
};
</script>

<style>
</style>
