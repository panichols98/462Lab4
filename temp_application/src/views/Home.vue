<template>
  <div class="home">
    <div class="current-temp">
      <h1>Current Reading: {{currentTemp}}&deg;F</h1>
      <button @click="loadData" class="temp-refresh-button">refresh</button>
    </div>

    <div class="my-columns">
      <div class="recent-temps">
        <h3>Recent Temperature Readings</h3>
        <ul>
          <li v-for="(reading, key) in recentReadings" :key="key">{{formatDate(key)}}: {{reading.temperatureF}}&deg;F</li>
        </ul>
      </div>

      <div class="violations">
        <h3>Violations</h3>
        <ul>
          <li v-for="(reading, key) in violations" :key="key">{{formatDate(key)}}: {{reading.temperatureF}}&deg;F</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import moment from 'moment'

export default {
  name: 'Home',
  data() {
    return {
      currentTemp: 0,
      recentReadings: [],
      violations: []
    }
  },
  async created() {
    await this.loadData();
  },
  methods: {
    async new_sensor_reading() {
      let url = "http://localhost:3000/sky/event/cl01w7gxy0ugec6px2abz3rk3/1556/emitter/new_sensor_reading"
      let response = await axios.post(url)
      console.log(response)
      this.currentTemp = response.data.directives[0].options.temp.temperatureF;
    },
    async get_recent_readings() {
      let url = "http://localhost:3000/sky/cloud/cl01w7gxy0ugec6px2abz3rk3/temperature_store/temps"
      let response = await axios.get(url);
      console.log(response);
      delete response.data.timestamp;
      this.recentReadings = response.data;
    },
    async get_violations() {
      let url = "http://localhost:3000/sky/cloud/cl01w7gxy0ugec6px2abz3rk3/temperature_store/threshold_violations"
      let response = await axios.get(url);
      console.log(response);
      delete response.data.timestamp;
      this.violations = response.data;
    },
    async loadData() {
      await this.new_sensor_reading();
      await this.get_recent_readings();
      await this.get_violations();
    },
    formatDate(date) {
      return moment(date).format('MMMM Do YYYY, h:mm:ss a')
    }
  }
}
</script>

<style scoped>
.current-temp {
  display: flex;
  justify-content: center;
  align-items: center;
}

.temp-refresh-button {
  margin-left: 15px;
  height: 50px;
}

li {
  list-style: none;
}

.my-columns {
  display: flex;
  justify-content: space-around;
}
</style>
