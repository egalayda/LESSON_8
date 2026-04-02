<script setup lang="ts">
import TheWelcome from '../components/TheWelcome.vue'
</script>

<script setup lang="ts">
import { Bar, Line, Doughnut } from 'vue-chartjs'
import {
  Chart as ChartJS,
  Title, Tooltip, Legend,
  BarElement, LineElement, PointElement,
  CategoryScale, LinearScale, ArcElement,
} from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, LineElement, PointElement, CategoryScale, LinearScale, ArcElement)

const barData = {
  labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
  datasets: [{ label: 'Revenue ($K)', backgroundColor: '#38bdf8', data: [42, 58, 35, 71, 63, 89] }],
}
const lineData = {
  labels: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
  datasets: [{ label: 'Active Users', borderColor: '#818cf8', backgroundColor: 'rgba(129,140,248,0.15)', fill: true, tension: 0.4, data: [120, 98, 145, 167, 134, 89, 201] }],
}
const doughnutData = {
  labels: ['Desktop', 'Mobile', 'Tablet'],
  datasets: [{ backgroundColor: ['#38bdf8', '#818cf8', '#34d399'], data: [55, 35, 10] }],
}
const chartOptions = { responsive: true, maintainAspectRatio: false, plugins: { legend: { position: 'top' as const } } }

const stats = [
  { icon: 'mdi-account-group', label: 'Total Users', value: '12,480', color: '#38bdf8' },
  { icon: 'mdi-currency-usd', label: 'Revenue', value: '$358K', color: '#34d399' },
  { icon: 'mdi-chart-line', label: 'Conversions', value: '3.6%', color: '#818cf8' },
  { icon: 'mdi-cart', label: 'Orders', value: '1,924', color: '#fb923c' },
]
</script>

<template>
  <div>
    <h1 class="page-title">Dashboard Overview</h1>

    <div class="stat-grid">
      <div v-for="stat in stats" :key="stat.label" class="stat-card">
        <div class="stat-icon" :style="{ background: stat.color + '22', color: stat.color }">
          <span class="mdi" :class="stat.icon"></span>
        </div>
        <div>
          <div class="stat-value">{{ stat.value }}</div>
          <div class="stat-label">{{ stat.label }}</div>
        </div>
      </div>
    </div>

    <div class="chart-grid">
      <div class="chart-card">
        <h2 class="chart-title"><span class="mdi mdi-chart-bar"></span> Monthly Revenue</h2>
        <div class="chart-wrap">
          <Bar :data="barData" :options="chartOptions" />
        </div>
      </div>
      <div class="chart-card">
        <h2 class="chart-title"><span class="mdi mdi-chart-line"></span> Active Users</h2>
        <div class="chart-wrap">
          <Line :data="lineData" :options="chartOptions" />
        </div>
      </div>
      <div class="chart-card chart-card--small">
        <h2 class="chart-title"><span class="mdi mdi-chart-donut"></span> Traffic by Device</h2>
        <div class="chart-wrap">
          <Doughnut :data="doughnutData" :options="chartOptions" />
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.page-title {
  font-size: 24px;
  font-weight: 700;
  margin-bottom: 24px;
  color: #1e293b;
}
.stat-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: 16px;
  margin-bottom: 28px;
}
.stat-card {
  background: #fff;
  border-radius: 12px;
  padding: 20px;
  display: flex;
  align-items: center;
  gap: 16px;
  box-shadow: 0 1px 4px rgba(0,0,0,0.07);
}
.stat-icon {
  width: 48px;
  height: 48px;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;
  flex-shrink: 0;
}
.stat-value { font-size: 22px; font-weight: 700; color: #1e293b; }
.stat-label { font-size: 12px; color: #64748b; margin-top: 2px; }
.chart-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: auto auto;
  gap: 20px;
}
.chart-card {
  background: #fff;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 1px 4px rgba(0,0,0,0.07);
}
.chart-card--small {
  grid-column: 1 / -1;
  max-width: 400px;
  justify-self: center;
  width: 100%;
}
.chart-title {
  font-size: 15px;
  font-weight: 600;
  color: #1e293b;
  margin-bottom: 16px;
  display: flex;
  align-items: center;
  gap: 8px;
}
.chart-title .mdi { color: #38bdf8; font-size: 18px; }
.chart-wrap { height: 250px; }
</style>
