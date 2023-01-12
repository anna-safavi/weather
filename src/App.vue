<template>
  <div id="app" :class="typeof weather.main != 'undefined' && overcast()">
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
          <div class="temp">{{ Math.round(weather.main.temp) }}°F</div>
          <div class="feels">
            Feels Like {{ Math.round(weather.main.feels_like) }}°F
          </div>
          <div class="weather">{{ weather.weather[0].main }}</div>
          <div class="info">
            <div class="details">Humidity {{ weather.main.humidity }} °F</div>
            <div class="details">
              Wind Speed {{ Math.round(weather.wind.speed) }} mph
            </div>
            <div class="details">
              Sunset {{ getSunTime(weather.sys.sunset) }}
            </div>
            <div class="details">
              Sunrise {{ getSunTime(weather.sys.sunrise) }}
            </div>
          </div>
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
        condition: "",
        api_key: process.env.VUE_APP_API_KEY,
        url_base: "https://api.openweathermap.org/data/2.5/",
        query: "",
        weather: {},
        date: "",
      };
    },
    methods: {
      fetchWeather(e) {
        if (e.key == "Enter") {
          fetch(
            `${this.url_base}weather?q=${this.query}&units=imperial&APPID=${this.api_key}`
          )
            .then((res) => {
              return res.json();
            })
            .then(this.setResults);
        }
      },
      getSunTime(unix) {
        let newtime = new Date(unix * 1000);
        let hours = newtime.getHours();
        let mins = "0" + newtime.getMinutes();
        let seconds = "0" + newtime.getSeconds();
        return (this.date =
          hours + ":" + mins.substr(-2) + ":" + seconds.substr(-2));
      },
      setResults(results) {
        this.weather = results;
        this.condition = this.weather.weather[0].main;
      },
      overcast() {
        if (this.condition == "Clear") {
          return "sunny";
        } else if (this.condition == "Clouds") {
          return "cloud";
        } else if (this.condition == "Fog") {
          return "fog";
        } else if (this.condition == "Snow") {
          return "snow";
        } else if (this.condition == "Rain") {
          return "rain";
        } else if (this.condition == "Mist") {
          return "mist";
        } else if (this.condition == "Drizzle") {
          return "drizzle";
        } else {
          return "";
        }
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
    background-image: url(./assets/images/suncloud.jpg);
    background-size: cover;
    background-position: bottom;
    transition: 0.4;
  }
  #app.cloud {
    background-image: url(./assets/images/clouds.jpg);
  }
  #app.sunny {
    background-image: url(./assets/images/sun.jpg);
  }
  #app.snow {
    background-image: url(./assets/images/snow.jpg);
  }
  #app.rain {
    background-image: url(./assets/images/rain.jpg);
  }
  #app.fog {
    background-image: url(./assets/images/fog.jpg);
  }
  #app.mist {
    background-image: url(./assets/images/mist.jpg);
  }
  #app.drizzle {
    background-image: url(./assets/images/drizzle.jpg);
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

  .weather-box {
    justify-content: center;
  }
  .weather-box .temp {
    display: inline-flex;
    color: white;
    padding: 10px 25px;
    width: 270px;
    height: 160px;
    font-size: 100px;
    font-weight: 900;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba(225, 225, 255, 0.5);
    border-radius: 16px;
    margin: 20px 0px;
  }

  .feels {
    color: white;
    padding: 10px 25px;
    font-size: 50px;
    font-weight: 400;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    margin: 10px 0px;
  }
  .weather-box .details {
    color: white;
    width: 350px;
    height: 60px;
    padding: 5px 5px;
    font-size: 24px;
    margin: 10px 10px;
    background-color: rgba(225, 225, 255, 0.25);
    border-radius: 16px;
  }
  .info {
    display: inline-block;
  }
  .weather-box .weather {
    color: white;
    font-size: 50px;
    font-weight: 700;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    font-style: italic;
  }
</style>
