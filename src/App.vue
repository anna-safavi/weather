<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search Location..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }} {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
  export default {
    name: "App",
    data() {
      return {
        api_key: "38198c6181c27beba9afdaf35119f73e",
        url_base: "https://api.openweathermap.org/data/2.5/",
        query: "",
        weather: {},
      };
    },
    methods: {
      fetchWeather(e) {
        if (e.key == "Enter") {
          fetch(
            `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
          )
            .then((res) => {
              return res.json();
            })
            .then(this.setResults);
        }
      },
      setResults(results) {
        this.weather = results;
      },
      dateBuilder() {
        let d = new Date();
        let months = [
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
        ];
        let days = [
          "Sunday",
          "Monday",
          "Tuesday",
          "Wednesday",
          "Thursday",
          "Friday",
          "Saturday",
        ];
        let day = days[d.getDay()];
        let date = d.getDate();
        let month = months[d.getMonth()];
        let year = d.getFullYear();
        return `${day} ${date} ${month} ${year}`;
      },
    },
  };
</script>

<style>
  * {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    text-align: center;
    color: #2c3e50;
    box-sizing: border-box;
  }
  body {
    font-family: Avenir, Helvetica, Arial, sans-serif;
  }
  #app {
    background-color: Blue;
    background-size: cover;
    background-position: bottom;
    transition: 0.4;
  }
  main {
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(
      to bottom rgba(0, 0, 0, 0.25),
      rgba(0, 0, 0, 0.75)
    );
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
    background: none;
    border: none;
    outline: none;
    background-color: rgba(225, 225, 255, 0.15);
    border-radius: 0px 16px 0px 16px;
    transition: 0.4s;
    box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  }

  .search-box .search-bar:focus {
    background-color: rgba(225, 225, 255, 0.75);
    box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
    border-radius: 0px 16px 0px 16px;
  }
  .location-box .location {
    color: white;
    font-size: 32px;
    font-weight: 500;
    font-style: italic;
    text-align: center;
  }
  .location-box .date {
    color: white;
    font-size: 32px;
    font-weight: 100;
    font-style: italic;
    text-align: center;
  }
  .weather-box .temp {
    display: inline-block;
    color: white;
    padding: 10px 25px;
    font-size: 100px;
    font-weight: 900;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba(225, 225, 255, 0.25);
    border-radius: 16px;
    margin: 30px 0px;
  }

  .weather-box .weather {
    color: white;
    font-size: 50px;
    font-weight: 700;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    font-style: italic;
  }
</style>
