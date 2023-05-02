<template>
  <v-card>
    <v-card-title> About </v-card-title>
    <v-card-text>
      This project was created by an aspiring web developer who is taking a class through Eastern
      Washington University - CSCD379<br />
      The end goal of this project is to create a working version of the classic word game Wordle
      using Vue.js and Vuetify. Along with a few other things to make it interesting.
      <br /><br />
      One of our instructors is Meg, she is a genius software engineer at IntelliTect. Shadowing her
      made me realize I wanted to become a developer! <br />
      The other instructor is Grant, and he is the Chief Technical Officer at IntelliTect.
    </v-card-text>
    <v-card-actions>
      <v-spacer />
      <v-img width="10%" :src="grant" />
      <v-spacer />
    </v-card-actions>
  </v-card>
  <br /><br />
  <v-btn class="button" variant="outlined" color="secondary" @click="$router.go(-1)">Back</v-btn>
  &nbsp;
  <v-btn variant="outlined" color="secondary" to="/">Home</v-btn>

  <v-spacer/> <br/>
  {{ isDialogOpen }}
  <WeatherDialog v-model="isDialogOpen" :weather="currentWeather"></WeatherDialog>

  <v-card v-for="item in weatherData" :key="item.date" @click="setCurrentWeather(item)">
    {{ item.summary }}</v-card
  >
</template>

<script setup lang="ts">
import grant from '@/assets/Grant.gif'
import Axios from 'axios'
import { ref } from 'vue'
import WeatherDialog from '@/components/WeatherDialog.vue'
import type { WeatherData } from '@/types/WeatherData'

const isDialogOpen = ref(false)
const weatherData = ref<WeatherData[]>()
const currentWeather = ref<WeatherData>()

function setCurrentWeather(weather: WeatherData) {
  currentWeather.value = weather
  isDialogOpen.value = true
}

Axios.get('https://localhost:7109/WeatherForecast')
  .then((response) => {
    console.log(response.data)
    weatherData.value = response.data
  })
  .catch((err) => {
    console.log(err)
  })
</script>

<script lang="ts">
export default {
  data() {
    return {
      dialog: false
    }
  }
}
</script>