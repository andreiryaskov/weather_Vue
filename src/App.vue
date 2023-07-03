<script>
import axios from 'axios'

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null,
    }
  },
  computed: {
    cityName() {
      return this.city
    },
    setTemp() {
      return this.info.main.temp
    },
    setFeelsLike() {
      return this.info.main.feels_like
    },
    maxTemp() {
      return this.info.main.temp_max
    },
    minTemp() {
      return this.info.main.temp_min
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Set more symbols"
        return false
      }
      this.error = ''

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=110da98b8ed50385ca1dfe9345d131d7`)
          .then(result => {
            console.log(result.data)
            this.info = result.data
          })
          .catch((error) => {
            console.log(error.message)
            this.error = error.message
          })
          .finally(() => console.log('END'))
    }
  }
}

</script>

<template>

  <div class="wrapper">
    <h1>Weather</h1>
    <p>Get weather {{ city == "" ? "your country" : cityName }}</p>
    <div class="form_wrapper">
      <input type="text"
             placeholder="Set country"
             v-model="city"
      >
      <button
          v-if="city != ''"
          @click="getWeather()"
      >Get weather
      </button>
      <button
          v-else="city"
          disabled
      >Set country
      </button>

    </div>


    <div class="result_wrapper">
      <p class="result_item" v-if="info !== null">Temperature is {{ setTemp }}</p>
      <p class="result_item" v-if="info !== null">Feels like is {{ setFeelsLike }}</p>
      <p class="result_item" v-if="info !== null">Max temperature is {{ maxTemp }}</p>
      <p class="result_item" v-if="info !== null">Min temperature is {{ minTemp }}</p>
    </div>

  </div>

</template>

<style scoped>

.result_item {
  color: antiquewhite;
  margin-top: 10px;
}

.result_wrapper {
  margin-top: 30px;
}

.form_wrapper {
  margin-top: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 50px;
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
  height: 30px;
  background: transparent;
  border: none;
  color: antiquewhite;
  padding: 5px;
  margin-right: 10px;

}

button {
  width: 100px;
  height: 30px;
  border-radius: 5px;
  background-color: burlywood;
}

button:disabled {
  opacity: 0.7;
}

</style>
