<template>
  <div
    id="app"
    :class="
      weatherData &&
      typeof weatherData.main != 'undefined' &&
      weatherData.main.temp > 15
        ? 'warm'
        : ''
    "
  >
    <h1></h1>
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div class="weather-wrap">
        <div class="location-box">
          <div class="location" v-if="weatherData">{{ weatherData.name }}</div>
          <div class="date" v-if="weatherData">
            {{ toLocaleDate(new Date()) }}
          </div>
          <div class="weather-box">
            <div
              class="temp"
              v-if="weatherData && typeof weatherData.main !== 'undefined'"
            >
              {{ `${Math.round(weatherData.main.temp)} °C` }}
            </div>
            <div class="weather" v-if="weatherData && weatherData.weather">
              {{ weatherData.weather[0].description }}
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import { api_key } from "./../api";
export default {
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((response) => {
            return response.json();
          })
          .then(this.setResults)
          .catch((error) => {
            console.log(error);
          });
      }
    },
    setResults(results) {
      this.weatherData = results;
    },
    toLocaleDate(value) {
      const options = {
        weekday: "long",
        year: "numeric",
        month: "long",
        day: "numeric",
      };
      return value.toLocaleDateString("de-DE", options);
    },
  },
  data() {
    return {
      api_key,
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weatherData: "",
    };
  },
  name: "Weather",
  components: {},
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
    Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
}
#app {
  background-image: url("./assets/cold-bg.png");
  background-size: cover;
  background-position: bottom;
  transition: 0.5s;
}
#app.warm {
  background-image: url("./assets/warm-bg.png");
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  display: block flow;
  color: #313131;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
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
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  font-size: 100px;
  color: #fff;
  font-weight: 900;
  text-shadow: 8px -5px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 8px -5px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 8px -5px rgba(0, 0, 0, 0.25);
  text-transform: capitalize;
}
</style>
