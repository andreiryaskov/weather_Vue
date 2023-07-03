<script >
import axios from 'axios'

import {computed} from "vue";

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null
    }
  },
  computed: {
    cityName() {
      return this.city
    }
  },
  methods: {
    getWeather() {
      if(this.city.trim().length < 2) {
        this.error = "Set more symbols"
        return false
      }
      this.error = ''

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=110da98b8ed50385ca1dfe9345d131d7`)
          .then(res => console.log(res.json()))
          .then(result => this.info = result.data)
          .catch((error) => console.log(error.message))
          .finally(() => console.log('END'))
    }
  }

}

</script>

<template>

  <div class="wrapper">
    <h1>Weather</h1>
    <p>Get weather {{city == "" ? "your country" : cityName}}</p>
    <input type="text"
           placeholder="Set country"
           v-model="city"
    >
    <button
      v-if="city != ''"
      @click="getWeather()"
    >Get weather</button>
    <button
        v-else="city"
        disabled
    >Set country</button>
    <p class="error">{{error}}</p>
    <p class="result" v-if="info !== null">{{info}}</p>
  </div>

</template>

<style scoped>

.result {
  color: antiquewhite;
}

.error {
  color: red;
}
.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background: black;
  text-align: center;
  color: white;
  margin-top: 100px;
  padding-top: 50px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

input {
  width: 200px;
  margin-top: 30px;
  background: transparent;
  border: none;
  color: antiquewhite;
  padding: 5px;
}

button {
  width: 100px;
  margin-top: 20px;
}

button:disabled {
  opacity: 0.7;
}

</style>
