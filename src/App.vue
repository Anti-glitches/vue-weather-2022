<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input type="text" v-model="query" @keypress.enter="fetchWeather" class="search-bar" placeholder="Search...">
      </div>

      <div class="weather-wrap" v-if="loaded">
        <div class="location-box">
          <div class="location">{{weather.name}}, {{weather.country}}</div>
          <div class="date">{{timestamp}}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{weather.temperature}}°C</div>
          <div class="weather">{{weather.condition}}</div>
        </div>
      </div>
      <div v-else>
        <p> please put in a city</p>
      </div>
    </main>
  </div>
</template>

<script lang="ts">
  import { defineComponent } from 'vue';
  import Weather from './types/Weather';
  import moment from 'moment';

  export default defineComponent({
    name:'App',
    data() {
      return {
        api_key: "ffc740c668d1aadb0fa5196a6ab6d73f",
        url_base: "https://api.openweathermap.org/data/2.5/",
        query: 'london',
        weather: {} as Weather,
        loaded: false,
      }
    },
    methods: {
      async fetchWeather(){
        const res = await fetch(`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
        const data = await res.json();

        const {name, sys: {country}, main: {temp}, weather:[{main}] } = data;

        this.weather = {
          name,
          country,
          temperature: Math.round(temp),
          condition: main
        }
        this.loaded = true;
      }
    },
    computed:{
      timestamp(){
        const date = new Date();

        return moment(date).format('MMMM Do, YYYY')
      }
    },
    created(){
      this.fetchWeather();
    }
  })
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: 'montserrat', sans-serif;
  width: 400px;
}
#app {
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  
  color: #313131;
  font-size: 20px;
  appearance: none;
  border:none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>