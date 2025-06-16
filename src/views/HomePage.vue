<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>График температуры</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content class="ion-padding">
      <h2>Температура: {{ currentTemperature }} °C</h2>
      <Temperature-chart/>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import axios from 'axios'
import TemperatureChart from '../components/TemperatureChart.vue'

const currentTemperature = ref('---')

const loadTemperature = async () => {
  try {
    const res = await axios.get('http://192.168.1.128:3000/temperature')
    currentTemperature.value = res.data.currentTemperature
  } catch (e) {
    console.error(e)
  }
}

onMounted(() => {
  loadTemperature()
  setInterval(loadTemperature, 5000)
})
</script>
