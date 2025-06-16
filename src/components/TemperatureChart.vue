<template>
  <ion-card>
    <Line :data="chartData" :options="chartOptions" />
  </ion-card>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import axios from 'axios'
import { Line } from 'vue-chartjs'
import {
  Chart as ChartJS, Title, Tooltip, Legend, LineElement, CategoryScale, LinearScale, PointElement
} from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, LineElement, CategoryScale, LinearScale, PointElement)

const chartData = ref({
  labels: [],
  datasets: [
    {
      label: 'Температура',
      backgroundColor: '#f87979',
      data: []
    }
  ]
})

const chartOptions = ref({
  responsive: true,
  maintainAspectRatio: false
})

onMounted(async () => {
  const res = await axios.get('http://192.168.1.128:3000/history')
  chartData.value.labels = res.data.map((r: any) =>
    new Date(r.timestamp).toLocaleTimeString()
  )
  chartData.value.datasets[0].data = res.data.map((r: any) => r.temperature)
})
</script>
