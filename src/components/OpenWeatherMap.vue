<template>
  <div id="main" :class="checkIfDay ? 'day' : 'night'">
    <img src="../assets/weather.jpg" class="weatherImage" />
    <h1>{{ msg }}</h1>
    <div class="container">
        <form class="search-location" v-on:submit.prevent="getWeather">
          <input
            type="text"
            class="form-control text-muted form-rounded p-4 shadow-sm"
            placeholder="Search for a place"
            v-model="cityNames"
            autocomplete="off"
          />
      </form>
      <p class="text-center notFound" v-if="cityFound">No city found</p>
      <div
        class="card rounded my-3 shadow-lg back-card overflow-hidden"
        v-if="visible"
      >
        <!-- weather animation container -->
        <div>
          <div icon="sunny" class="clearSky" v-if="clearSky">
            <img src="../assets/sunny.gif" />
          </div>

          <div icon="snowy" v-if="snowy">
            <img src="../assets/stormy.gif" />
          </div>

          <div icon="rainy" v-if="rainy">
            <img src="../assets/rain.gif" />
          </div>

          <div icon="stormy" v-if="stormy">
            <img src="../assets/stormy.gif" />
          </div>
          <div icon="cloudy" v-if="cloudy">
            <img src="../assets/cloudy.gif" />
          </div>
          <div icon="nightmoon" v-if="clearNight">
            <img src="../assets/night.gif" />
          </div>
        </div>

        <div class="card-top text-center" style="margin-bottom: 15rem">
          <div class="city-name my-3">
            <p>{{ weather.cityName }}</p>
            <p class="">Country: {{ weather.country }}</p>
          </div>
         </div>
        <div class="card-body">
          <div class="card-mid">
            <div class="row">
              <div class="col-12 text-center temp">
                <span>{{ weather.temperature }}&deg;C</span>
                <p class="my-4">{{ weather.description }}</p>
              </div>
            </div>
            <div class="row">
              <div class="col d-flex justify-content-between px-5 mx-5">
                <p>
                  Min: {{ weather.lowTemp }}&deg;C
                </p>
                <p>
                  Max: {{ weather.highTemp }}&deg;C
                </p>
              </div>
            </div>
            <div class="row">
            <p class="wind"> Wind: {{ weather.wind }}km/h </p>
            </div>
          </div>
          <div class="card-bottom px-5 py-4 row">
            <div class="col text-center">
              <p>{{ weather.feelsLike }}&deg;C</p>
              <span>Feels like</span>
            </div>
            <div class="col text-center">
              <p>{{ weather.humidity }}%</p>
              <span>humidity</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'OpenWeatherMap',
  data () {
    return {
      cityFound: false,
      visible: false,
      stormy: false,
      cloudy: false,
      rainy: false,
      clearSky: false,
      clearNight: false,
      snowy: false,
      checkIfDay: true,
      cityNames: "",
      weather: {
        cityName: "",
        country: "",
        temperature: 0,
        description: "",
        lowTemp: "",
        highTemp: "",
        feelsLike: "",
        humidity: "",
        wind: "",
      },
    };
  },
  methods: {
      // async function getWeather() {
      getWeather: async function() {
        console.log("---> bllaa", this.cityNames);
        const weatherAPI = 'ca72b6ea7aee294a1c96bb8631b1f7a4';
        const weatherURL = `https://api.openweathermap.org/data/2.5/weather?q=${this.cityNames}&appid=${weatherAPI}&units=metric`;

        try {
          const response = await fetch(weatherURL);
          const data = await response.json();
          this.cityNames = "";
          console.log("-->", data);
          this.weather.cityName = data.name;
          this.weather.country = data.sys.country;
          this.weather.temperature = Math.round(data.main.temp);
          this.weather.description = data.weather[0].description;
          this.weather.lowTemp = Math.round(data.main.temp_min);
          this.weather.highTemp = Math.round(data.main.temp_max);
          this.weather.feelsLike = Math.round(data.main.feels_like);
          this.weather.humidity = Math.round(data.main.humidity);
          this.weather.wind = (data.wind.speed);

          const timeOfDay = data.weather[0].icon;
          if (timeOfDay.includes("n")) {
            this.checkIfDay = false;
          } else {
            this.checkIfDay = true;
          }
          const mainWeather = data.weather[0].main;
          if (mainWeather.includes("Clouds")) {
            this.stormy = false;
            this.cloudy = true;
            this.clearSky = false;
            this.clearNight = false;
            this.snowy = false;
          }
          if (mainWeather.includes("Clouds")) {
            this.stormy = false;
            this.cloudy = true;
            this.clearSky = false;
            this.clearNight = false;
            this.snowy = false;
            this.rainy = false;

          }
          if (
            mainWeather.includes("Thunderstorm")
          ) {
            this.stormy = true;
            this.cloudy = false;
            this.clearSky = false;
            this.clearNight = false;
            this.rainy = false;
            this.snowy = false;
          }
          if (mainWeather.includes("Rain")) {
            this.rainy = true;
            this.stormy = false;
            this.cloudy = false;
            this.clearSky = false;
            this.clearNight = false;
            this.snowy = false;

          }
          if (mainWeather.includes("Clear") && this.checkIfDay) {
            this.stormy = false;
            this.cloudy = false;
            this.clearSky = true;
            this.clearNight = false;
            this.snowy = false;
          }
          if (mainWeather.includes("Clouds") && !this.checkIfDay) {
            this.stormy = false;
            this.cloudy = false;
            this.clearSky = false;
            this.clearNight = true;
            this.snowy = false;
          }
          if (mainWeather.includes("Snow")) {
            this.stormy = false;
            this.cloudy = false;
            this.clearSky = false;
            this.clearNight = false;
            this.snowy = true;
          }
          this.visible = true;
          this.cityFound = false;
        } catch (error) {

          console.log(error);
          this.cityFound = true;
          this.visible = false;
        }
      }
  },
  props: {
    msg: String
  }
};
</script>

<style scoped>
  @import "../assets/icons.css";
  @import "../assets/custom.css";
</style>
