<template>
  <div class="profile">
    <h1>Edit and Save the Profile of this Sensor!</h1>
    <div class="my-box">
      <div class="field">
        <label for="location">Location: </label>
        <input v-model="location" id="location">
      </div>

      <div class="field">
        <label for="name">Name: </label>
        <input v-model="name" id="name">
      </div>

      <div class="field">
        <label for="phone">Phone Number: </label>
        <input v-model="phone" id="phone">
      </div>

      <div class="field">
        <label for="threshold">Threshold: </label>
        <input v-model="threshold" id="threshold">
      </div>

      <button @click="saveProfile()" class="save-button">Save</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Profile ',
  data() {
    return {
      location: '',
      name: '',
      phone: '',
      threshold: ''
    }
  },
  async created() {
    let url = "http://localhost:3000/sky/cloud/cl01w7gxy0ugec6px2abz3rk3/sensor_profile/profile"
    let response = await axios.get(url);
    this.location = response.data.location;
    this.name = response.data.name;
    this.phone = response.data.phone;
    this.threshold = response.data.threshold;
  },
  methods: {
    async saveProfile() {
      let url = "http://localhost:3000/sky/event/cl01w7gxy0ugec6px2abz3rk3/1556/sensor/profile_updated"
      let response = await axios.post(url, {
        location: this.location,
        name: this.name,
        phone: this.phone,
        threshold: this.threshold
      })

      if (response.status == 200) {
        alert("profile saved");
      }
    }
  }
}
</script>

<style scoped>
.my-box {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
}

.field {
  display: flex;
  justify-content: space-between;
  width: 300px;
  margin-top: 20px;
}

.save-button {
  margin-top: 10px;
}
</style>
