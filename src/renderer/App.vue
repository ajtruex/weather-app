<template>
  <main id="app">
    <p>City name:</p>
    <section class="weather-input">
      <input type="text" v-model="query">
      <button :disabled="!query.length" @click="showWeather">Check</button>
    </section>
    <section v-if="error" class="weather-error">
      No city in DB
    </section>
    <section v-if="city.length" class="weather-result">
      <h1>{{city}}, {{country}}</h1>
      <p><em>{{weatherDescription}}</em></p>
      <div class="weather-result__main">
        <img :src="icon" alt="Weather icon">
        <div class="weather-result__temp">
          {{temp}}&deg;F
        </div>
      </div>
      <div class="weather-result__details">
        <p>Min: {{tempMin}}&deg;F</p>
        <p>Max: {{tempMax}}&deg;F</p>
        <p>Humidity: {{humidity}}%</p>
      </div>
    </section>
  </main>
</template>

<script>
import LandingPage from "@/components/LandingPage"

export default {
	name: "weather-app",
	data() {
		return {
			query: "",
			error: false,
			city: "",
			country: "",
			weatherDescription: "",
			temp: null,
			tempMin: null,
			tempMax: null,
			humidity: null,
			icon: ""
		}
	},
	methods: {
		showWeather() {
			this.$http
				.get(
					`/weather?q=${
						this.query
					}&units=imperial&&appid=ba39c059b49a61b8e75e165e7efdda2b`
				)
				.then(response => {
					this.city = response.data.name
					this.country = response.data.sys.country
					this.weatherDescription =
						response.data.weather[0].description
					this.temp = response.data.main.temp
					this.tempMin = response.data.main.temp_min
					this.tempMax = response.data.main.temp_max
					this.humidity = response.data.main.humidity
					this.icon = `http://openweathermap.org/img/w/${
						response.data.weather[0].icon
					}.png`
					this.error = false
				})
				.catch(() => {
					this.error = true
					this.city = ""
				})
		}
	}
}
</script>

<style lang="scss">
* {
	margin: 0;
	padding: 0;
}

html,
body,
#app {
	height: 100%;
}

#app {
	font-family: Arial, Helvetica, sans-serif;
	font-size: 16px;
	padding: 10px;
	background: rgb(212, 228, 239);
	background: -moz-radial-gradient(
		center,
		ellipse cover,
		rgba(212, 228, 239, 1) 0%,
		rgba(134, 174, 204, 1) 100%
	);
	background: -webkit-radial-gradient(
		center,
		ellipse cover,
		rgba(212, 228, 239, 1) 0%,
		rgba(134, 174, 204, 1) 100%
	);
	background: radial-gradient(
		ellipse at center,
		rgba(212, 228, 239, 1) 0%,
		rgba(134, 174, 204, 1) 100%
	);
	filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#d4e4ef', endColorstr='#86aecc', GradientType=1);
	/* IE6-9 fallback on horizontal gradient */
}

.weather-input {
	display: flex;
	align-items: center;
	padding: 20px 0;
}

.weather-result {
	text-align: center;

	&__main {
		display: flex;
		align-items: center;
		justify-content: center;
		padding-top: 5px;
		font-size: 1.3rem;
		font-weight: bold;
	}

	&__details {
		display: flex;
		align-items: center;
		justify-content: space-around;
		color: dimgray;
	}
}

.weather-error {
	color: red;
	font-weight: bold;
}

input {
	width: 75%;
	outline: none;
	height: 20px;
	font-size: 0.8rem;
}

button {
	display: block;
	width: 25%;
	height: 25px;
	outline: none;
	border-radius: 5px;
	white-space: nowrap;
	margin: 0 10px;
	font-size: 0.8rem;
}
</style>