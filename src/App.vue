<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp <= 25
        ? 'cold'
        : 'warm'
    "
  >
    <main>
      <div class="header">
        <h2>Weather App</h2>
        <h2>Want to Know the Weather, We are Here</h2>
      </div>
      <div class="searchbox">
        <input
          type="text"
          name="searchbar"
          id="searchbar"
          class="searchbar"
          placeholder="Search..."
          @keypress="getweather"
          v-model="query"
        />
      </div>
      <div class="weather" v-if="typeof weather.main != 'undefined'">
        <div class="location">
          <p>{{ this.weather.name }}</p>
          <span>{{ this.weather.sys.country }}</span>
        </div>
        <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
        <div class="weather-icon">
          <img :src="icon" alt="" />
        </div>
        <div class="condition">
          {{weather.weather[0].main}}
        </div>
      </div>
      <div class="additional" v-if="typeof weather.main != 'undefined'">
        <table>
          <td>
            <tr>
              Visibility
            </tr>
            <tr>
              Humidity
            </tr>
            <tr>
              Wind Speed
            </tr>
            <tr>
              Pressure
            </tr>
            <tr>
              Maximum Temperature
            </tr>
            <tr>
              Minimum Temperature
            </tr>
          </td>

          <td>
            <tr>
              {{
                this.weather.visibility
              }}
              meters
            </tr>
            <tr>
              {{
                this.weather.main.humidity
              }}%
            </tr>
            <tr>
              {{
                this.weather.wind.speed
              }}
              meter/sec
            </tr>
            <tr>
              {{
                this.weather.main.pressure
              }}
              h/Pa
            </tr>
            <tr>
              {{
                this.weather.main.temp_max
              }}°C
            </tr>
            <tr>
              {{
                this.weather.main.temp_min
              }}°C
            </tr>
          </td>
        </table>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      api_key: "13c16fb318cfeeae7a3fb2a577a31d14",
      url: "https://api.openweathermap.org/data/2.5/weather",
      query: "",
      weather: {},
      icon: "",
    };
  },
  methods: {
    getweather(e) {
      if (e.key === "Enter") {
        fetch(`${this.url}?&q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then((res) => {
            return res.json();
          })
          .then(this.setresponse)
          .then(this.geticon);
      }
    },
    setresponse(res) {
      this.weather = res;
      console.log(this.weather);
    },
    geticon() {
      this.icon = `http://openweathermap.org/img/wn/${this.weather.weather[0].icon}@2x.png`;
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  background-image: url("./assets/wp2035106.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
  height: 100vh;
}
#app.cold {
  background-image: url("./assets/2942776.jpg");
}
#app.warm {
  background-image: url("./assets/warm.jpg");
}
main {
  font-family: "Poppins", sans-serif;
  display: flex;
  flex-direction: column;
  color: white;
  padding-top: 20px;
  min-height: 100vh;
  align-items: center;
  background-image: linear-gradient(rgb(0, 0, 0, 0.25), rgb(0, 0, 0, 0.75));
}
.header {
  text-align: center;
  font-family: "Poppins", sans-serif;
  margin-bottom: 5px;
}
<!-- Styling for the SearchBox > .searchbox {
  padding: 35px;
}
.searchbar {
  appearance: none;
  outline: none;
  border: none;

  height: 30px;
  padding: 20px;
  margin-bottom: 5px;

  background-color: lightgrey;
  border-radius: 8px;
  transition: 0.5s;
}
.searchbox:focus {
  box-shadow: rgb(0, 0, 0, 0.5);
  border-radius: 10px 0px 10px 0px;
}

.location {
  margin: 5px;
}
.location p {
  text-align: center;
  font-size: 35px;
}
.location span {
  text-align: center;
  font-size: 25px;
}
.temp {
  text-align: center;
  font-size: 40px;
}
.condition {
  text-align: center;
  font-size: 25px;
}
table {
  margin: auto;
}
table td {
  padding: 20px;
}
table tr {
  font-size: 18px;
  padding-bottom: 5px;
}
</style>
