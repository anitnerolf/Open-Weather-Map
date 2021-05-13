<template>
  <div class="weather">
    <h1>{{ msg }}</h1>
    <div>
        <form class="search-location" v-on:submit.prevent="getWeather">
        <input
          type="text"
          class="form-control text-muted form-rounded p-4 shadow-sm"
          placeholder="What City?"
          v-model="citySearch"
          autocomplete="off"
        />
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'OpenWeatherMap',
  data () {
    return {
        cityNames: "",
        citySearch: "",
        weather: {
        },
    };
  },
  methods: {
      // async function getWeather() {
      getWeather: async function() {
        console.log(this.cityNames);
        // const weatherAPI = 'ca72b6ea7aee294a1c96bb8631b1f7a4';
        const weatherAPI = "6a093b7057e5a7dc9fb69f84e336ac5c";
        const weatherURL = `https://api.openweathermap.org/data/2.5/weather?q=${this.cityNames}&appid=${weatherAPI}&units=metric`;

        try {
          const response = await fetch(weatherURL);
          const data = await response.json();
          console.log(data);
          this.cityNames = "";
        } catch (error) {
          console.log(error);
        }
      }
  },
  props: {
    msg: String
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.weather {
    color: blue;
    font-size: 20px;
}

</style>
