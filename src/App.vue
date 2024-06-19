<template>
  <div id="app">
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
      sampleData: [
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
        },
        {
          "strike_price": 103, 
          "type": "Put", 
          "bid": 14, 
          "ask": 15.50, 
          "long_short": "short", 
          "expiration_date": "2025-12-17T00:00:00Z"
        },
        {
          "strike_price": 105, 
          "type": "Put", 
          "bid": 16, 
          "ask": 18, 
          "long_short": "long", 
          "expiration_date": "2025-12-17T00:00:00Z"
        }
      ]
    }
  }
}


</script>

<style>
#app {
  font-family: "Fira Sans", sans-serif;
  font-weight: 400;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin:60px 10vw;
}
.fira-sans-regular {
  font-family: "Fira Sans", sans-serif;
  font-weight: 400;
  font-style: normal;
}

.fira-sans-bold {
  font-family: "Fira Sans", sans-serif;
  font-weight: 700;
  font-style: normal;
}

.fira-sans-black {
  font-family: "Fira Sans", sans-serif;
  font-weight: 900;
  font-style: normal;
}
h1,h2,h3,h4,h5{ font-weight: 900;}
strong,b{ font-weight: 700; }
</style>
