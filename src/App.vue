<template>
  <div id="app"> <option-input-form @generate-graph="handleGenerateGraph" /> <risk-reward-graph v-if="graphData"
      :chart-data="graphData.chartData" :max-profit="graphData.maxProfit" :max-loss="graphData.maxLoss"
      :break-even-points="graphData.breakEvenPoints" /> </div>
</template>
<script>
import OptionInputForm from './components/OptionInputForm.vue'; import RiskRewardGraph from './components/RiskRewardGraph.vue'; export default { name: 'App', components: { OptionInputForm, RiskRewardGraph, }, data() { return { graphData: null, }; }, methods: { handleGenerateGraph(options) { const result = this.calculateRiskReward(options); this.graphData = result; }, calculateRiskReward(options) { let maxProfit = -Infinity; let maxLoss = Infinity; let breakEvenPoints = []; const data = { labels: [], datasets: [{ data: [], label: 'Profit/Loss' }] }; const prices = []; for (let price = 0; price <= 200; price += 1) { prices.push(price); } prices.forEach((price) => { let totalPnl = 0; options.forEach((option) => { const strike = parseFloat(option.strike); const premium = parseFloat(option.premium); if (option.type === 'call') { totalPnl += Math.max(0, price - strike) - premium; } else if (option.type === 'put') { totalPnl += Math.max(0, strike - price) - premium; } }); data.labels.push(price); data.datasets[0].data.push(totalPnl); maxProfit = Math.max(maxProfit, totalPnl); maxLoss = Math.min(maxLoss, totalPnl); if (totalPnl === 0) { breakEvenPoints.push(price); } }); return { chartData: { labels: data.labels, datasets: [{ label: 'Profit/Loss', data: data.datasets[0].data, borderColor: 'rgba(75, 192, 192, 1)', backgroundColor: 'rgba(75, 192, 192, 0.2)', fill: false, },], }, maxProfit, maxLoss, breakEvenPoints, }; }, }, }; </script>
<style>
 #app {
   font-family: Avenir, Helvetica, Arial, sans-serif;
   -webkit-font-smoothing: antialiased;
   -moz-osx-font-smoothing: grayscale;
   text-align: center;
   color: #2c3e50;
   margin-top: 60px;
 }
</style>