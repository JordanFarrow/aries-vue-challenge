<template>
  <div>
    <h1>Options Profit Calculator</h1>
    Data <pre>{{ optionsData }}</pre>
    <Line :data="data" :options="options" />
  </div>
</template>

<script>
import {
    Chart as ChartJS,
    Title,
    Tooltip,
    Legend,
    BarElement,
    CategoryScale,
    LinearScale
  } from 'chart.js'
  import { Line } from 'vue-chartjs'
  ChartJS.register(CategoryScale, LinearScale, BarElement, Title, Tooltip, Legend)

export default {
  name: 'CodingChallenge',
  props: {
    optionsData: Array
  },
  components:{
    Line
  },
  data() {
      return {
        data: {
          labels: ['January', 'February', 'March'],
          datasets: [{ data: [40, 20, 12] }]
        },
        options: {
          responsive: true
        }
      }
    }
  // Your code here
}
</script>

<style scoped>

</style>
