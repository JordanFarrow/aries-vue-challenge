<template>
  <div>
    <h1>Options Profit Calculator</h1>
    <pre>    {{ chartOptions }}</pre>
    <LineChart :data="data" :options="chartOptions" />
    <OptionsCard :cardData="optionsData" ref="cards" />

  </div>
</template>

<script>
const optionContractQty = 100;
const unlimitedX = 1.5; // used to plot graph for unlimited gain/loss 
const palette = ['rgba(0, 63, 92, 1)', 'rgba(0, 129, 155, 1)', 'rgba(0, 196, 148, 1)', 'rgba(126, 255, 66, 1)'];

import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
} from 'chart.js';
import { Line as LineChart } from 'vue-chartjs';

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
)
import OptionsCard from './OptionsCard.vue';

export default {
  name: 'CodingChallenge',
  props: {
    optionsData: [],
    chartOptions:{
      type: Object,
      default: ()=>{}
    }
  },
  components: {
    LineChart,
    OptionsCard
  },
  data() {
    //process optionsData for max p/l, be for each
    //options contract represents 100 of underlying asset

    let xAxisLabels = [];
    let lineDatasets = [];
    for (let od = 0; od < this.optionsData.length; od++) {
      let contract = this.optionsData[od];
      xAxisLabels.push(contract.strike_price + "");
      // buy or sell?
      let type = contract.type;
      if (contract.long_short === "long") {
        if (type === "Call") {
          //long call
          contract.maxProfit = contract.strike_price * unlimitedX;
          contract.maxLoss = contract.bid * optionContractQty;
          contract.breakEven = contract.strike_price + contract.bid;
          contract.lineData = [{ x: contract.strike_price - 5, y: -contract.bid }, { x: contract.strike_price, y: -contract.bid }, { x: contract.strike_price, y: 0 }, { x: contract.strike_price + 5, y: contract.maxProfit }];
        } else {
          //long Put
          contract.maxProfit = contract.strike_price - contract.bid;
          contract.maxLoss = contract.ask;
          contract.breakEven = contract.strike_price - contract.ask;
          contract.lineData = [{ x: contract.strike_price - 5, y: contract.ask }, { x: contract.strike_price, y: contract.ask }, { x: contract.strike_price, y: 0 }, { x: contract.strike_price + 5, y: contract.maxLoss }];

        }
      } else {
        if (type === "Put") {
          //short put
          contract.maxProfit = contract.bid;
          contract.maxLoss = contract.strike_price * unlimitedX;
          contract.breakEven = contract.strike_price - contract.bid;
          contract.lineData = [{ x: contract.strike_price - 5, y: contract.ask }, { x: contract.strike_price, y: contract.ask }, { x: contract.strike_price, y: 0 }, { x: contract.strike_price + 5, y: contract.maxLoss }];

        } else {
          //short call option
          contract.maxProfit = contract.bid;
          contract.maxLoss = (contract.strike_price + contract.bid) - contract.ask;
          contract.breakEven = contract.strike_price + contract.bid;
          contract.lineData = [{ x: contract.strike_price - 5, y: -contract.bid }, { x: contract.strike_price, y: -contract.bid }, { x: contract.strike_price, y: 0 }, { x: contract.strike_price + 5, y: contract.maxProfit }];

        }
      }
      contract.label = `${type} option (${contract.long_short})`;
      contract.color = palette[od];

      lineDatasets.push({
        label: contract.label,
        fill: false,
        borderColor: contract.color,
        data: contract.lineData
      });
    }
 
    return {
      data: {
        type: 'line',
        labels: xAxisLabels,
        datasets: lineDatasets,
        chartOptions: {
          scales: {
            yAxes: [{
              scaleLabel: {
                display: true,
                labelString: 'Profit/Loss'
              }
            }],
            y: {
              maxTicksLimit: 10 // Limit the number of ticks on the y-axis
            },
            x: {
              maxTicksLimit: 10 // Limit the number of ticks on the x-axis
            }
          },
          interaction: {
            intersect: false,
          }
        },
        optionsData: this.optionsData,
        unlimitedX: this.unlimitedX
      }
    }
  }
}
</script>

<style scoped></style>
