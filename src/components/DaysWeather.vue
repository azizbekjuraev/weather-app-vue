<script>
import axios from "axios";
import moment from "moment";
export default (await import("vue")).defineComponent({
  name: "DaysWeather",
  props: {
    cityname: String,
  },
  data() {
    return {
      apiData: "",
      loaded: false,
    };
  },
  mounted() {
    this.fetchWeatherData();
  },
  methods: {
    async fetchWeatherData() {
      const apiKey = `da83202889dafd3e4717272b971e8e57`;
      const city = this.cityname;
      const apiUrl = `https://api.openweathermap.org/data/2.5/forecast?q=${city}&units=metric&appid=${apiKey}`;
      await axios
        .get(apiUrl)
        .then((res) => {
          const forecastData = res.data.list;
          const filteredData = forecastData
            .map((item) => {
              return {
                date: moment(item.dt_txt.split(" ")[0]),
                temperature: Math.round(item.main.temp),
                description: item.weather[0].description,
                iconUrl: `https://api.openweathermap.org/img/w/${item.weather[0].icon}.png`,
              };
            })
            .reduce((acc, item) => {
              if (!acc.some((day) => day.date.isSame(item.date, "day"))) {
                acc.push(item);
              }
              return acc;
            }, [])
            .slice(1, 5);
          this.apiData = filteredData;
          this.loaded = true;
        })
        .catch((error) => {
          console.error(error);
          this.loading = false;
        });
    },
  },
});
</script>

<template>
  <div class="days-tab text-center">
    <div class="loading" v-if="!loaded">Loading...</div>
    <ul class="p-0">
      <li class="li_active" v-for="data in apiData">
        <div class="p-3"><img :src="data.iconUrl" alt="" /></div>
        <div class="p-3">{{ data.date._i }}</div>
        <div class="p-3">{{ data.temperature }}&deg</div>
      </li>
    </ul>
  </div>
</template>
<style>
.days-tab {
  width: 90%;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  margin: auto;
}
.loading {
  color: #fff;
}
ul {
  margin: 0;
}
li {
  display: inline-block;
  list-style: none;
  height: 100%;
  width: 21%;
  font-size: 1vw;
  line-height: 1.2;
}
span {
  display: block;
  margin-bottom: 5px;
  font: 100% sans-serif;
  height: 35px;
}
.li_active {
  background: #253d5c;
  color: #222831;
  border-radius: 10px;
  margin: 0.5rem;
  color: #fff;
  font-weight: 600;
}
.li_active:hover {
  transform: scale(1.1);
  transition: transform 0.1s ease;
}
.li_active_temp {
  display: inline-block;
  background-color: #222831;
  color: #fff;
  transition: background-color 0.5s;
  border-radius: 10px;
}
</style>
