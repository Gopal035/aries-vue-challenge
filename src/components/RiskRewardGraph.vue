<template>
  <div>
    <canvas ref="canvas"></canvas>
  </div>
</template>

<script>
import { ref, onMounted, watch } from 'vue';
import { Chart, registerables } from 'chart.js';

Chart.register(...registerables);

export default {
  name: 'RiskRewardGraph',
  props: {
    graphData: {
      type: Object,
      required: true
    }
  },
  setup(props) {
    const canvas = ref(null);
    let chart = null;

    const renderChart = () => {
      if (chart) chart.destroy();

      chart = new Chart(canvas.value, {
        type: 'line',
        data: props.graphData,
        options: {
          responsive: true,
          plugins: {
            legend: {
              position: 'top'
            },
            title: {
              display: true,
              text: 'Risk & Reward Graph'
            }
          },
          scales: {
            x: {
              title: {
                display: true,
                text: 'Underlying Price at Expiry'
              }
            },
            y: {
              title: {
                display: true,
                text: 'Profit/Loss'
              }
            }
          }
        }
      });
    };

    onMounted(() => {
      renderChart();
    });

    watch(() => props.graphData, () => {
      renderChart();
    });

    return {
      canvas
    };
  }
};
</script>

<style scoped>
</style>
