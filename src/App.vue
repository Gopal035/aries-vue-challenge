<template>
  <div id="app">
  <div>
      <button @click="setSampleData(1)">Scenario 1</button>
      <button @click="setSampleData(2)">Scenario 2</button>
      <button @click="setSampleData(3)">Scenario 3</button>
    </div>
    <CodingChallenge :optionsData="sampleData"/>
  </div>
</template>

<script>
import CodingChallenge from './components/CodingChallenge.vue'

export default {
  name: 'App',
  components: {
    CodingChallenge
  },
  data() {
    return {
       scenarios: {
        1: [
          {
            "strike_price": 100, 
            "type": "Call", 
            "bid": 10.05, 
            "ask": 12.04, 
            "long_short": "long", 
            "expiration_date": "2025-12-17T00:00:00Z"
          },
          {
            "strike_price": 102.50, 
            "type": "Call", 
            "bid": 12.10, 
            "ask": 14, 
            "long_short": "long", 
            "expiration_date": "2025-12-17T00:00:00Z"
          }
        ],
        2: [
          {
            "strike_price": 95, 
            "type": "Put", 
            "bid": 8, 
            "ask": 9.5, 
            "long_short": "short", 
            "expiration_date": "2025-12-17T00:00:00Z"
          },
          {
            "strike_price": 98, 
            "type": "Call", 
            "bid": 10, 
            "ask": 11.5, 
            "long_short": "long", 
            "expiration_date": "2025-12-17T00:00:00Z"
          },
          {
            "strike_price": 105, 
            "type": "Put", 
            "bid": 5, 
            "ask": 6, 
            "long_short": "long", 
            "expiration_date": "2025-12-17T00:00:00Z"
          }
        ],
        3: [
          {
            "strike_price": 110, 
            "type": "Call", 
            "bid": 15, 
            "ask": 16.5, 
            "long_short": "short", 
            "expiration_date": "2025-12-17T00:00:00Z"
          },
          {
            "strike_price": 115, 
            "type": "Put", 
            "bid": 12, 
            "ask": 13.5, 
            "long_short": "long", 
            "expiration_date": "2025-12-17T00:00:00Z"
          }
        ]
      },
      sampleData: []
    }
  },
  methods: {
    setSampleData(scenario) {
      this.sampleData = this.scenarios[scenario];
    }
  },
  created() {
    this.setSampleData(1);
  }
}
</script>

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
