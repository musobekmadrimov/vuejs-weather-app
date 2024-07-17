<script>
import axios from "axios";
export default {
  data(){
    return {
      city: "",
      error: "",
      info: null
    }
  },
  computed: {
    cityName(){
      return '"' + this.city + '"';
    },
    showTemp(){
      return this.info.main.temp + ' °C';
    },
    showFeelsLike(){
      return this.info.main.feels_like + ' °C';
    },
    showMinTemp(){
      return this.info.main.temp_min + ' °C';
    },
    showMaxTemp(){
      return this.info.main.temp_max + ' °C';
    },
    showGeneralWeatherStatus(){
      return this.info.weather[0].main;
    },
    showPressure(){
      return this.info.main.pressure;
    },
    showHumidity(){
      return this.info.main.humidity;
    },
  },
  methods: {
    getWeather(){
      const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=1dac6e4f0758eccd4f9e3396b7adea43&units=metric`;
      if(this.city.trim().length < 2){
        this.error = "Please enter a city name.";
        return false;
      }
      this.error = "";
      axios.get(apiUrl)
      .then(response => {
        console.log(response.data);
        this.info = response.data;
      });
    }
  },
  watch: {
    city(newVal) {
      if (newVal === '') {
        this.info = null
      }
    }
  },
}
</script>



<template>
  <div class="wrapper">
    <h1>Musobek's Weather App on VueJS</h1>
    <p>for showing the weather in <span id="city">{{ city === "" ? "your city" : cityName }}</span></p>
    <input type="text" v-model="city" name="city" id="city" placeholder="Enter your city" />
    <button v-if="city !== ''" @click="getWeather()">Show the weather</button>
    <button class="disabled-button" disabled v-else>Enter your city</button>
    <p class="error">{{ error }}</p>
    <div class="answer" v-if="this.info !== null">
      <p><span class="bold">Weather: </span>{{ showGeneralWeatherStatus }}</p>
      <p><span class="bold">Temperature: </span>{{ showTemp }}</p>
      <p><span class="bold">Feels like: </span>{{ showFeelsLike }}</p>
      <p><span class="bold">The lowest temperature: </span>{{ showMinTemp }}</p>
      <p><span class="bold">The highest temperature: </span>{{ showMaxTemp }}</p>
      <p><span class="bold">Pressure: </span>{{ showPressure }}</p>
      <p><span class="bold">Humidity: </span>{{ showHumidity }}</p>
    </div>

  </div>

</template>

<style scoped>

  .wrapper {
    width: 900px;
    height: 500px;
    border-radius: 50px;
    padding: 20px;
    background: #1f0f24;
    text-align: center;
    color: #fff;
  }
  .wrapper h1 {
    margin-top: 50px;
  }

  .wrapper p {
    margin-top: 10px;
    margin-bottom: 20px;
  }

  .wrapper p span {
    font-size: 20px;
    font-weight: bold;
  }

  .wrapper input {
    margin-top: 30px;
    background: transparent;
    border: 0;
    border-bottom: 1px solid #6e2d7d;
    color: #fcfcfc;
    font-size: 14px;
    padding: 5px 8px;
    outline: none;
    transition: all 0.3s ease-in-out;
  }

  .wrapper input:focus {
    border-bottom: 3px solid #6e2d7d;
  }

  .wrapper button {
    background: #6e2d7d;
    color: #fff;
    font-weight: 900;
    border-radius: 10px;
    border: 2px solid #6e2d7d;
    padding: 10px 15px;
    margin-left: 20px;
    cursor: pointer;
    outline: none;
    transition: transform 0.5s ease;
  }

  .wrapper button:hover {
    transform: scale(1.1) translateY(-5px);
  }

  .wrapper button.disabled-button {
    cursor: not-allowed;
    background: #51205a;
  }

  .wrapper p.error {
    color: #d03939;
  }

  .answer {
    width: 50%;
    margin-inline: auto;
    text-align: left;
  }

  .answer p {
    display: flex;
    justify-content: space-between;
    margin: 0;
  }

  p.bold {
    font-weight: bold;
  }

</style>
