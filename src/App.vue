<script>
export default {
  name: 'vueweatherapp',
  data() {
    return {
      api_key: '5943558cdc52c4552978fba9c0ad9ade',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
          return res.json();})
          .then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
      this.query = '';
    },
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<template>

<!-- tuka bakcground kalau warm  -->
  <div id="vueweatherapp" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input type="text" 
        class="search-bar" 
        placeholder="Search..." 
        v-model="query"
        @keypress="fetchWeather"
        />
      </div>
<!-- kalau takde type dia takkan keluar detail jadi kita guna v-if -->
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">
            {{ Math.round(weather.main.temp) }}°c
          </div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>

  </div>
</template>




<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
}

#vueweatherapp {
  background-image: url('./assets/background.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#vueweatherapp.warm {
  background-image: url('./assets/warm.jpg');
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
  font-size: 1.2rem;

  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 10px 10px 10px 10px;
  transition: 0.4s;
}

/* bila tekan dia berubah warna so kita target focus  */
.search-box .search-bar:focus {
  background-color: rgba(255, 255, 255, 0.8);
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.5);
  /* ada transition tukar shape box  */
  border-radius: 0px 16px 0px 16px;
}

.location-box  .location{
  color: aliceblue;
  font-size: 2rem;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: aliceblue;
  font-size: 1.5rem;
  font-weight: 300;
  text-align: center;
  font-style: italic;
  /* text-shadow: 1px 3px rgba(0, 0, 0, 0.25); */
}

.weather-box {
 text-align: center;
  
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: aliceblue;
  font-size: 6.3rem;
  font-weight: 900;
  text-shadow: rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 10px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);


}

.weather-box .weather {
  color: aliceblue;
  font-size: 3rem;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
