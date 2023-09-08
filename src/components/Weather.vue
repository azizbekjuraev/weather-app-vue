<script>
import axios from "axios";
import DaysWeather from "./DaysWeather.vue";
export default (await import("vue")).defineComponent({
  name: "myWeather",
  components: { DaysWeather },
  props: {
    city: String,
    searchWeather: Function,
  },

  data() {
    return {
      cityname: this.city,
      temperature: null,
      description: null,
      iconUrl: null,
      date: null,
      time: null,
      name: null,
      country: null,
      see_level: null,
      wind: null,
      humidity: null,
      monthNames: [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ],
      dayNames: [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ],
      day: null,
    };
  },
  async created() {
    const response = await axios.get(
      `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=da83202889dafd3e4717272b971e8e57`
    );
    const weatherData = response.data;
    this.temperature = Math.round(weatherData.main.temp);
    this.description = weatherData.weather[0].description;
    this.iconUrl = `https://api.openweathermap.org/img/w/${weatherData.weather[0].icon}.png`;
    const d = new Date();
    this.time = d.getHours() + ":" + d.getMinutes() + ":" + d.getSeconds();
    this.name = weatherData.name;
    this.date =
      d.getDate() + "-" + this.monthNames[d.getMonth()] + "-" + d.getFullYear();
    this.day = this.dayNames[d.getDay()];
    this.country = weatherData.sys.country;
    this.wind = weatherData.wind.speed;
    this.see_level = weatherData.main.pressure;
    this.humidity = weatherData.main.humidity;
  },
});
</script>

<template>
  <div class="container p-0">
    <div class="d-flex gap-2">
      <div class="card main-div w-100">
        <div class="p-3">
          <h2 class="mb-1 day">{{ day }}</h2>
          <p class="text-light date mb-0">{{ date }}</p>
          <small>{{ time }}</small>
          <h2 class="place">
            <i class="fa-solid fa-location-crosshairs"></i>
            {{ name }}, <small>{{ country }}</small>
          </h2>
          <div class="temp">
            <h1 class="weather-temp">{{ temperature }}&deg;</h1>
            <h2 class="text-light">{{ description }} <img :src="iconUrl" /></h2>
          </div>
        </div>
      </div>
      <div class="card card-2 w-100">
        <table class="m-4">
          <tbody>
            <tr>
              <th>See level</th>
              <td>{{ see_level }}</td>
            </tr>
            <tr>
              <th>Wind</th>
              <td>{{ wind }}</td>
            </tr>
            <tr>
              <th>Humidity</th>
              <td>{{ humidity }}</td>
            </tr>
          </tbody>
        </table>
        <DaysWeather :cityname="cityname"></DaysWeather>
        <div id="div_Form" class="d-flex m-3 justify-content-center">
          <form action="">
            <input
              @click="searchWeather"
              type="button"
              value="Change Location"
              class="btn change-btn btn-primary"
            />
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
body {
  background-color: #343d4b;
}
.weather-temp {
  font-weight: 700;
  font-size: 4em;
  margin: 0;
}
h2.mb-1.day {
  font-size: 3rem;
  font-weight: 400;
}
.main-div {
  border-radius: 20px;
  background-image: url("https://images.pexels.com/photos/36717/amazing-animal-beautiful-beautifull.jpg?auto=compress&cs=tinysrgb&w=800");
  color: #fff;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  background-color: rgba(0, 0, 0, 0.5);
}
.temp {
  position: absolute;
  bottom: 0;
}
.main-div:hover {
  transform: scale(1.1);
  transition: transform 0.5s ease;
  z-index: 1;
}
.card-2 {
  background-color: #212730;
  border-radius: 20px;
}
.card-details {
  margin-left: 19px;
}
.h1_left {
  position: absolute;
  bottom: 25px;
  left: 16px;
  font-size: 3vw;
  line-height: 1.2;
}
.h3_left {
  position: absolute;
  left: 16px;
  font-size: 2vw;
  line-height: 0.5;
}
.h3_left small {
  font-size: 1rem;
}
table {
  position: relative;
  left: 15px;
  border-collapse: separate;
  border-spacing: 15px;
  width: 85%;
  text-align: left;
  max-width: 600px;
  margin: 0 auto;
}
th,
td {
  font-size: 18px;
  color: #fff;
}
td {
  text-align: right;
}
table,
tr:hover {
  color: red;
}
.change-btn {
  background-image: linear-gradient(to right, cyan, magenta);
}
.change-btn:hover {
  transform: scale(0.9);
  transition: transform 0.1s ease;
}
</style>
