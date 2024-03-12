<script>
import { computed } from 'vue';
import axios from 'axios'

export default {
  data() {
    return {
      city: '',
      error: '',
      info: null
    }
  },
  computed: {
      cityName() {
        return '"' + this.city + '"'
      },
      showCondition() {
        return 'Condition: ' + this.info.current.condition.text
      },
      showTemp() {
        return 'Temperature ' + this.info.current.temp_c + ' C'
      },
      showFeelsLike() {
        return 'Feels like ' + this.info.current.feelslike_c + ' C'
      },
      showWindSpeed() {
        return 'Wind speed ' + this.info.current.wind_kph + ' km/h'
      }
    },
    methods: {
      getWeather() {
        if(this.city.trim().length < 2) {
          this.error = "the city must be longer of one letter..."
          return false

        }
        this.error = ''

        axios.get(`http://api.weatherapi.com/v1/current.json?key=42d5b67a0af94883968180027232510&q=${this.city}`)
          .then(res => this.info = res.data) 
      }
    }
}
</script>

<template>
  <div class="wrapper">
    <h1>Weather</h1>
    <p class="subTitle">Weather in {{ city == "" ? 'your city' : cityName }}</p>
    <div class="inputBox">
      <input type="text" v-model="city" placeholder="Text your city here">
    <button v-if="city != '' " @click="getWeather()">ok</button> 
    <button disabled v-else="city != '' ">ok</button> 
    </div>
    <p class="error">{{ error }}</p> 
    <div v-if="info != null" class="dataBox">
      <p class="data">{{ showCondition }}</p>
      <p class="data">{{ showTemp }}</p>
      <p class="data">{{ showFeelsLike }}</p>
      <p class="data">{{ showWindSpeed }}</p>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  width: 70vw;
  height: 60vh;
  min-height: 300px ;
  border-radius: 48px;
  padding: 48px;
  background: hsla(0, 0%, 20%, 0.2);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
}

.wrapper h1 {
  text-align: center;
  color: #fff;
  margin-bottom: 24px;
  font-size: 48px;
  font-weight: 700;
}

.subTitle {
  color: #fff;
  font-size: 24px;
  text-align: center;
  font-weight: 400;
}

.inputBox {
  width: 100%;
  display: flex;
  justify-content: center;
  padding-top: 48px;
}

input {
  width: 200px;
  height: 48px;
  padding-left: 24px;
  border-radius: 24px 0 0 24px;
  border: 0;
  outline: none;
  font-size: 16px;
}

button {
  padding: 0 24px;
  height: 48px;
  border-radius: 0 24px 24px 0;
  background-color: rgba(0, 0, 0, 0.75);
  color: #fff;
  border: 0;
  cursor: pointer;
  transition: 0.1s ease;
}

button:hover {
  background-color: rgba(0, 0, 0, 0.9);
}

button:disabled {
  background-color: rgba(0, 0, 0, 0.6);
  cursor: default;
}

.error {
  font-size: 16px;
  color: #cc0e0ee2;
  text-align: center;
  padding-top: 12px;
}

.dataBox {
  width: 250px;
  margin: 0 auto;
}

.data {
  padding-top: 12px; 
  font-size: 20px;
  font-weight: 400;
  color: #fff;
}

</style>
