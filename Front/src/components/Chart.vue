<template>
  <div id="data-chart">
    <Doughnut
      ref="chart"
      width="400"
      height="300"
      id="my-chart-id"
      :options="chartOptions"
      :data="chartData"
      :key="chartKey"
    />
  </div>
</template>

<script>
import { Doughnut } from "vue-chartjs";
import { Chart as ChartJS, Title, Tooltip, Legend, ArcElement } from "chart.js";
import axios from "axios";

ChartJS.register(Title, Tooltip, Legend, ArcElement);
ChartJS.defaults.plugins.legend.position = "right";

export default {
  name: "ChartComponent",
  components: {
    Doughnut,
  },
  data() {
    return {
      chartKey: 0,
      chartData: {
        labels: [],
        datasets: [
          {
            data: [],
            backgroundColor: [],
          },
        ],
      },
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false,
        plugins: {
          legend: {
            display: true,
            position: "right",
            labels: {
              usePointStyle: true,
              pointStyle: "rect",
            },
          },
        },
      },
    };
  },
  watch: {
    chartData: {
      handler() {
        this.chartKey += 1;
      },
      deep: true,
    },
  },
  mounted() {
    this.fetchDataFromAPI();
  },
  methods: {
    async fetchDataFromAPI() {
      try {
        const response = await axios.get(
          "http://localhost:3000/values"
        );
        const apiData = response.data;

        const labels = apiData.map(
          (item) => `${item.firstname} ${item.lastname}`
        );
        const data = apiData.map((item) => item.participation);
        const backgroundColor = apiData.map(() => this.randomHexColor());

        this.chartData.labels = labels;
        this.chartData.datasets[0].data = data;
        this.chartData.datasets[0].backgroundColor = backgroundColor;
      } catch (error) {
        console.error("Error fetching data from API:", error);
      }
    },
    randomHexColor() {
      const color = Math.floor(Math.random() * 16777215).toString(16);
      return `#${"0".repeat(6 - color.length)}${color.toUpperCase()}`;
    },
  },
};
</script>