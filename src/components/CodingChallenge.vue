<template>
  <div>
    <RiskRewardGraph :graphData="graphData" />
  </div>
</template>

<script>
import { ref, onMounted, watch } from 'vue';
import RiskRewardGraph from './RiskRewardGraph.vue';

export default {
  name: 'CodingChallenge',
  components: {
    RiskRewardGraph
  },
  props: {
    optionsData: {
      type: Array,
      required: true
    }
  },
  setup(props) {
    const graphData = ref({});

    const calculateGraphData = (options) => {
      const pricesAtExpiry = Array.from({ length: 200 }, (_, i) => i);

      const profitLossData = pricesAtExpiry.map(price => {
        let totalProfitLoss = 0;

        options.forEach(option => {
          const { strike_price, type, bid, ask, long_short } = option;
          const premium = (bid + ask) / 2;

          if (type === 'Call') {
            if (price > strike_price) {
              const payoff = price - strike_price;
              totalProfitLoss += (long_short === 'long' ? payoff - premium : premium - payoff);
            } else {
              totalProfitLoss += (long_short === 'long' ? -premium : premium);
            }
          } else if (type === 'Put') {
            if (price < strike_price) {
              const payoff = strike_price - price;
              totalProfitLoss += (long_short === 'long' ? payoff - premium : premium - payoff);
            } else {
              totalProfitLoss += (long_short === 'long' ? -premium : premium);
            }
          }
        });

        return totalProfitLoss;
      });

      return {
        labels: pricesAtExpiry,
        datasets: [
          {
            label: 'Profit/Loss',
            data: profitLossData,
            borderColor: 'rgba(75, 192, 192, 1)',
            borderWidth: 2,
            fill: false
          }
        ]
      };
    };

    onMounted(() => {
      graphData.value = calculateGraphData(props.optionsData);
    });

    watch(() => props.optionsData, (newData) => {
      graphData.value = calculateGraphData(newData);
    });

    return {
      graphData
    };
  }
};
</script>

<style scoped>
</style>
