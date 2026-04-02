<script setup lang="ts">
import { ref, computed } from 'vue'
import { Bar, Line } from 'vue-chartjs'
import {
  Chart as ChartJS,
  Title, Tooltip, Legend, Filler,
  BarElement, LineElement, PointElement,
  CategoryScale, LinearScale,
} from 'chart.js'
import metrics from './data/metrics.json'

ChartJS.register(Title, Tooltip, Legend, Filler, BarElement, LineElement, PointElement, CategoryScale, LinearScale)

const months = ['ALL', ...metrics.map(m => m.month)]
const selectedMonth = ref('ALL')

const filtered = computed(() => {
  if (selectedMonth.value === 'ALL') return metrics
  return metrics.filter(m => m.month === selectedMonth.value)
})

const prevMonth = computed(() => {
  if (selectedMonth.value === 'ALL') return null
  const idx = metrics.findIndex(m => m.month === selectedMonth.value)
  return idx > 0 ? metrics[idx - 1] : null
})

const summaryRevenue = computed(() =>
  selectedMonth.value === 'ALL'
    ? metrics.reduce((s, m) => s + m.revenue, 0)
    : filtered.value[0]?.revenue ?? 0
)
const summaryVisitors = computed(() =>
  selectedMonth.value === 'ALL'
    ? metrics.reduce((s, m) => s + m.visitors, 0)
    : filtered.value[0]?.visitors ?? 0
)
const summaryConversions = computed(() => {
  const arr = filtered.value
  return +(arr.reduce((s, m) => s + m.conversions, 0) / arr.length).toFixed(1)
})
const summaryOrders = computed(() =>
  selectedMonth.value === 'ALL'
    ? metrics.reduce((s, m) => s + m.orders, 0)
    : filtered.value[0]?.orders ?? 0
)

function delta(key: 'revenue' | 'visitors' | 'conversions' | 'orders') {
  if (selectedMonth.value === 'ALL' || !prevMonth.value) return null
  const curr = filtered.value[0]?.[key] ?? 0
  const prev = prevMonth.value[key]
  if (prev === 0) return null
  return +(((curr - prev) / prev) * 100).toFixed(1)
}

const palette = {
  primary: '#5C6BC0',
  secondary: '#26A69A',
  accent: '#FF7043',
}

const barData = computed(() => ({
  labels: filtered.value.map(m => m.month),
  datasets: [{
    label: 'Revenue ($)',
    backgroundColor: palette.primary,
    data: filtered.value.map(m => m.revenue),
  }],
}))

const lineData = computed(() => ({
  labels: filtered.value.map(m => m.month),
  datasets: [{
    label: 'Visitors',
    borderColor: palette.secondary,
    backgroundColor: 'rgba(38,166,154,0.15)',
    fill: true,
    tension: 0.4,
    data: filtered.value.map(m => m.visitors),
  }],
}))

const areaData = computed(() => ({
  labels: filtered.value.map(m => m.month),
  datasets: [{
    label: 'Conversion %',
    borderColor: palette.accent,
    backgroundColor: 'rgba(255,112,67,0.15)',
    fill: true,
    tension: 0.4,
    data: filtered.value.map(m => m.conversions),
  }],
}))

const chartOpts = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: { legend: { labels: { color: '#ccc' } } },
  scales: {
    x: { ticks: { color: '#aaa' }, grid: { color: '#333' } },
    y: { ticks: { color: '#aaa' }, grid: { color: '#333' } },
  },
}

function fmtDollar(n: number) {
  return '$' + n.toLocaleString()
}
function fmtNum(n: number) {
  return n.toLocaleString()
}
</script>

<template>
  <v-app>
    <v-app-bar color="surface" density="comfortable" elevation="2">
      <v-app-bar-title>
        <v-icon class="mr-2">mdi-view-dashboard</v-icon>
        My Dashboard
      </v-app-bar-title>
      <template #append>
        <v-select
          v-model="selectedMonth"
          :items="months"
          variant="outlined"
          density="compact"
          hide-details
          style="max-width: 160px"
          label="Month"
        />
      </template>
    </v-app-bar>

    <v-main>
      <v-container fluid class="pa-6">
        <v-row>
          <v-col cols="12" sm="6" md="3">
            <v-card class="pa-4" rounded="lg">
              <div class="d-flex align-center">
                <v-avatar color="indigo-darken-1" size="48" class="mr-4">
                  <v-icon>mdi-currency-usd</v-icon>
                </v-avatar>
                <div>
                  <div class="text-h6 font-weight-bold">{{ fmtDollar(summaryRevenue) }}</div>
                  <div class="text-caption text-medium-emphasis">Revenue</div>
                  <div v-if="delta('revenue') !== null" class="text-caption" :class="delta('revenue')! >= 0 ? 'text-success' : 'text-error'">
                    <v-icon size="14">{{ delta('revenue')! >= 0 ? 'mdi-arrow-up' : 'mdi-arrow-down' }}</v-icon>
                    {{ Math.abs(delta('revenue')!) }}%
                  </div>
                </div>
              </div>
            </v-card>
          </v-col>
          <v-col cols="12" sm="6" md="3">
            <v-card class="pa-4" rounded="lg">
              <div class="d-flex align-center">
                <v-avatar color="teal-darken-1" size="48" class="mr-4">
                  <v-icon>mdi-account-group</v-icon>
                </v-avatar>
                <div>
                  <div class="text-h6 font-weight-bold">{{ fmtNum(summaryVisitors) }}</div>
                  <div class="text-caption text-medium-emphasis">Visitors</div>
                  <div v-if="delta('visitors') !== null" class="text-caption" :class="delta('visitors')! >= 0 ? 'text-success' : 'text-error'">
                    <v-icon size="14">{{ delta('visitors')! >= 0 ? 'mdi-arrow-up' : 'mdi-arrow-down' }}</v-icon>
                    {{ Math.abs(delta('visitors')!) }}%
                  </div>
                </div>
              </div>
            </v-card>
          </v-col>
          <v-col cols="12" sm="6" md="3">
            <v-card class="pa-4" rounded="lg">
              <div class="d-flex align-center">
                <v-avatar color="deep-orange-darken-1" size="48" class="mr-4">
                  <v-icon>mdi-percent</v-icon>
                </v-avatar>
                <div>
                  <div class="text-h6 font-weight-bold">{{ summaryConversions }}%</div>
                  <div class="text-caption text-medium-emphasis">Conversions</div>
                  <div v-if="delta('conversions') !== null" class="text-caption" :class="delta('conversions')! >= 0 ? 'text-success' : 'text-error'">
                    <v-icon size="14">{{ delta('conversions')! >= 0 ? 'mdi-arrow-up' : 'mdi-arrow-down' }}</v-icon>
                    {{ Math.abs(delta('conversions')!) }}%
                  </div>
                </div>
              </div>
            </v-card>
          </v-col>
          <v-col cols="12" sm="6" md="3">
            <v-card class="pa-4" rounded="lg">
              <div class="d-flex align-center">
                <v-avatar color="deep-purple-darken-1" size="48" class="mr-4">
                  <v-icon>mdi-cart</v-icon>
                </v-avatar>
                <div>
                  <div class="text-h6 font-weight-bold">{{ fmtNum(summaryOrders) }}</div>
                  <div class="text-caption text-medium-emphasis">Orders</div>
                  <div v-if="delta('orders') !== null" class="text-caption" :class="delta('orders')! >= 0 ? 'text-success' : 'text-error'">
                    <v-icon size="14">{{ delta('orders')! >= 0 ? 'mdi-arrow-up' : 'mdi-arrow-down' }}</v-icon>
                    {{ Math.abs(delta('orders')!) }}%
                  </div>
                </div>
              </div>
            </v-card>
          </v-col>
        </v-row>

        <v-row class="mt-4">
          <v-col cols="12" md="6">
            <v-card class="pa-4" rounded="lg">
              <div class="text-subtitle-1 font-weight-bold mb-3">
                <v-icon class="mr-1" color="indigo">mdi-chart-bar</v-icon>
                Monthly Revenue
              </div>
              <div style="height: 280px">
                <Bar :data="barData" :options="chartOpts" />
              </div>
            </v-card>
          </v-col>
          <v-col cols="12" md="6">
            <v-card class="pa-4" rounded="lg">
              <div class="text-subtitle-1 font-weight-bold mb-3">
                <v-icon class="mr-1" color="teal">mdi-chart-line</v-icon>
                Visitors Over Time
              </div>
              <div style="height: 280px">
                <Line :data="lineData" :options="chartOpts" />
              </div>
            </v-card>
          </v-col>
        </v-row>

        <v-row class="mt-4">
          <v-col cols="12">
            <v-card class="pa-4" rounded="lg">
              <div class="text-subtitle-1 font-weight-bold mb-3">
                <v-icon class="mr-1" color="deep-orange">mdi-chart-areaspline</v-icon>
                Conversions Trend
              </div>
              <div style="height: 250px">
                <Line :data="areaData" :options="chartOpts" />
              </div>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>
