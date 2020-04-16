<template>
  <div id="home" :class="main.temp > 16 ? 'warm' : ''">
    <main class="row">
      <div class="search-box col-md-3 mt-5">
        <div class="input-group search-bar mt-5">
          <select @change="fetchWeather" class="mt-2 mb-2 custom-select" v-model="city">
            <option disabled value>Seçiniz...</option>
            <option v-for="city in cities" :key="city">{{city}}</option>
          </select>
        </div>
      </div>
      <div class="weather-wrap col-md-8" v-if="select">
        <div class="location-box">
          <div class="location">{{ cityName }} {{ sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">
            {{ main.temp }}°C
            <img :src="iconSrc+weatherArr.icon" alt />
          </div>
          <div class="weather">{{ weatherArr.description }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "home",
  data() {
    return {
      select: false,
      api_key: "37135e7c61342d0aed2565712a89d950",
      base_url: "http://api.openweathermap.org/data/2.5/weather?q=",
      city: "",
      cityName: "",
      weatherArr: [],
      sys: {},
      main: {},
      cities: []
    };
  },
  computed: {
    iconSrc() {
      return "http://api.openweathermap.org/img/w/";
    }
  },
  created() {
    this.fetchCities();
  },
  methods: {
    //Cities Json
    fetchCities() {
      axios.get("http://localhost:8080/cities.json").then(res => {
        this.cities = res.data;
      });
    },

    //Weather Json
    fetchWeather() {
      axios
        .get(
          `${this.base_url}${this.city}&lang=tr&units=metric&appid=${this.api_key}`
        )
        .then(res => {
          this.select = true;
          this.sys = res.data.sys;
          this.main = res.data.main;
          this.weatherArr = res.data.weather[0];
          this.cityName = res.data.name;
        });
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "Ocak",
        "Şubat",
        "Mart",
        "Nisan",
        "Mayıs",
        "Haziran",
        "Temmuz",
        "Ağustos",
        "Eylül",
        "Ekim",
        "Kasım",
        "Aralık"
      ];
      let days = [
        "Pazartesi",
        "Salı",
        "Çarşamba",
        "Perşembe",
        "Cuma",
        "Cumartesi",
        "Pazar"
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day}-${date}-${month}-${year}`;
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}
#home {
  background-image: url(/src/assets/cold-bg.jpg);
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#home.warm {
  background-image: url(/src/assets/warm-bg.jpg);
}
main {
  min-height: 100vh;
  padding: 25px;
  margin-right: 0 !important;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}
img {
  width: 150px;
  height: 100px;
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
  border: none;
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
  font-style: italic;
  text-align: center;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
