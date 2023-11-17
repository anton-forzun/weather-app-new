<template>
  <div class="container" v-if="temperatureData">
    <canvas ref="temperatureChart"></canvas>
  </div>
</template>
  
  <script>
import Chart from "chart.js/auto";

export default {
  props: ["temperatureData"],
  data() {
    return {
      chartInstance: null,
    };
  },
  // mounted() {
  //   if (this.temperatureData) {
  //     this.renderChart();
  //   }
  // },
  watch: {
    temperatureData: "renderChart",
  },
  methods: {
    destroyChart() {
      if (this.chartInstance) {
        this.chartInstance.destroy();
      }
    },
    renderChart() {
      if (!this.temperatureData || !this.$refs.temperatureChart) {
        return;
      }

      this.destroyChart();

      const ctx = this.$refs.temperatureChart.getContext("2d");

      this.chartInstance = new Chart(ctx, {
        type: "line",
        data: {
          labels: this.temperatureData.map((hour) => hour.time),
          datasets: [
            {
              label: "Temperature",
              borderColor: "rgb(75, 192, 192)",
              data: this.temperatureData.map((hour) => hour.temperature),
            },
          ],
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          scales: {
            x: {
              type: "linear",
              position: "bottom",
            },
          },
        },
      });
    },
  },
  beforeDestroy() {
    this.destroyChart();
  },
};
</script>
  
  <style scoped>
.container {
  margin: 20px 0;
}
canvas {
  height: 300px;
}
</style>