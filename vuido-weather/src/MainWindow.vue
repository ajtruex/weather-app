<template>
  <Window title="vuido-weather" width="400" height="150" margined v-on:close="exit">
    <Box padded>
    <Box horizontal padded>
      <TextInput stretchy v-model="query"/>
      <Button :enabled="!!query" @click="showWeather">Search</Button>
    </Box>
    <Separator horizontal/>
    <Group margined>
      <Box padded>
        <Text v-if="error">No city</Text>
        <Box v-if="!!city">
        <Box padded horizontal>
          <Text stretchy>{{city}}, {{country}}</Text>
          <Text>{{temp}}&deg;F</Text>
        </Box>
        <Text>{{weatherDescription}}</Text>
        <Separator horizontal/>
        <Box padded horizontal>
          <Text stretchy>Min: {{tempMin}}&deg;F</Text>
          <Text stretchy>Max: {{tempMax}}&deg;F</Text>
          <Text stretchy>Humidity: {{humidity}}%</Text>
        </Box>
        </Box>
      </Box>
    </Group>
    </Box>
  </Window>
</template>

<script>
import axios from "axios";
import dotenv from "dotenv/config";
axios.defaults.baseURL = "http://api.openweathermap.org/data/2.5";
// const apiKey = process.env.API_KEY;

// console.log(this.query);

export default {
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
			humidity: null
		};
	},
	methods: {
		exit() {
			this.$exit();
		},
		showWeather() {
			axios
				.get(
					`/weather?q=${
						this.query
					}&units=imperial&&appid=ba39c059b49a61b8e75e165e7efdda2b`
				)
				.then(response => {
					this.city = response.data.name;
					this.country = response.data.sys.country;
					this.weatherDescription =
						response.data.weather[0].description;
					this.temp = response.data.main.temp;
					this.tempMin = response.data.main.temp_min;
					this.tempMax = response.data.main.temp_max;
					this.humidity = response.data.main.humidity;
					this.error = false;
				})
				.catch(() => {
					this.error = true;
					this.city = "";
				});
		}
	}
};
</script>
