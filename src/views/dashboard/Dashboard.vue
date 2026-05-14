<template>
  <div class="min-h-screen bg-gradient-to-br from-gray-50 to-gray-100 p-6">
    <!-- Espace pour header (si nécessaire) -->
    <div class="h-20"></div>

    <!-- STATISTIQUES avec icônes et dégradés -->
    <section class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6 mb-10">
      <div class="bg-gradient-to-br from-orange-600 to-orange-500 text-white p-5 rounded-2xl shadow-lg hover:shadow-xl transition-transform transform hover:-translate-y-1">
        <div class="flex justify-between items-start">
          <div>
            <h2 class="text-sm font-medium opacity-80">Revenus totaux</h2>
            <p class="text-3xl font-bold mt-2">$628</p>
            <p class="text-xs opacity-75 mt-1">+12% vs mois dernier</p>
          </div>
          <i class='bx bx-dollar-circle text-3xl opacity-70'></i>
        </div>
      </div>

      <div class="bg-white p-5 rounded-2xl shadow-md border-b-4 border-orange-500 hover:shadow-lg transition-all">
        <div class="flex justify-between items-start">
          <div>
            <h2 class="text-sm text-gray-500">Partages</h2>
            <p class="text-3xl font-bold text-gray-800 mt-2">2 434</p>
            <p class="text-xs text-green-500">+8%</p>
          </div>
          <i class='bx bx-share-alt text-2xl text-orange-400'></i>
        </div>
      </div>

      <div class="bg-white p-5 rounded-2xl shadow-md border-b-4 border-orange-500 hover:shadow-lg transition-all">
        <div class="flex justify-between items-start">
          <div>
            <h2 class="text-sm text-gray-500">J'aime</h2>
            <p class="text-3xl font-bold text-gray-800 mt-2">1 259</p>
            <p class="text-xs text-green-500">+5%</p>
          </div>
          <i class='bx bx-heart text-2xl text-orange-400'></i>
        </div>
      </div>

      <div class="bg-white p-5 rounded-2xl shadow-md border-b-4 border-orange-500 hover:shadow-lg transition-all">
        <div class="flex justify-between items-start">
          <div>
            <h2 class="text-sm text-gray-500">Note moyenne</h2>
            <p class="text-3xl font-bold text-gray-800 mt-2">8.5</p>
            <p class="text-xs text-orange-500">/10</p>
          </div>
          <i class='bx bx-star text-2xl text-orange-400'></i>
        </div>
      </div>
    </section>

    <!-- GRAPHIQUES améliorés -->
    <section class="grid grid-cols-1 lg:grid-cols-3 gap-6">
      <!-- BAR CHART avec ombre et transitions -->
      <div class="bg-white p-6 rounded-2xl shadow-md hover:shadow-xl transition-all duration-300 lg:col-span-2">
        <div class="flex justify-between items-center mb-6">
          <h2 class="text-lg font-semibold text-gray-700 flex items-center gap-2">
            <i class='bx bx-trending-up text-orange-500'></i>
            Évolution des résultats
          </h2>
          <button class="bg-orange-500 hover:bg-orange-600 text-white px-4 py-1.5 rounded-full text-sm font-medium transition">
            Voir plus
          </button>
        </div>
        <canvas ref="barChart" width="400" height="200" style="max-height: 300px;"></canvas>
      </div>

      <!-- DONUT CHART modernisé avec légende personnalisée -->
      <div class="bg-white p-6 rounded-2xl shadow-md flex flex-col items-center hover:shadow-xl transition-all">
        <h2 class="text-lg font-semibold text-gray-700 mb-4 text-center">Satisfaction client</h2>
        <div class="relative w-48 h-48">
          <canvas ref="donutChart" width="200" height="200"></canvas>
          <div class="absolute inset-0 flex flex-col items-center justify-center">
            <span class="text-3xl font-bold text-orange-500">45%</span>
            <span class="text-xs text-gray-500">satisfaits</span>
          </div>
        </div>
        <div class="flex gap-4 mt-4 text-sm">
          <div class="flex items-center gap-1"><span class="w-3 h-3 rounded-full bg-orange-500"></span> Satisfait</div>
          <div class="flex items-center gap-1"><span class="w-3 h-3 rounded-full bg-gray-300"></span> Non satisfait</div>
        </div>
      </div>

      <!-- LINE CHART avec dégradé et ombre -->
      <div class="bg-white p-6 rounded-2xl shadow-md lg:col-span-2 hover:shadow-xl transition-all">
        <h2 class="text-lg font-semibold text-gray-700 mb-4 flex items-center gap-2">
          <i class='bx bx-line-chart text-orange-500'></i>
          Performance hebdomadaire
        </h2>
        <canvas ref="lineChart" width="400" height="200" style="max-height: 260px;"></canvas>
      </div>

      <!-- LISTE DES VISITEURS avec design amélioré -->
      <div class="bg-white p-6 rounded-2xl shadow-md hover:shadow-xl transition-all">
        <div class="flex justify-between items-center mb-4">
          <h2 class="text-lg font-semibold text-gray-700 flex items-center gap-2">
            <i class='bx bx-group text-orange-500'></i>
            Visiteurs récents
          </h2>
          <span class="text-xs bg-gray-100 px-2 py-1 rounded-full text-gray-600">{{ visitors.length }} total</span>
        </div>
        <ul class="divide-y divide-gray-100">
          <li v-for="visitor in visitors" :key="visitor" class="py-3 flex items-center gap-3">
            <div class="w-8 h-8 rounded-full bg-gradient-to-br from-orange-200 to-orange-400 flex items-center justify-center text-white font-semibold text-sm">
              {{ visitor.charAt(0) }}
            </div>
            <span class="text-gray-700">{{ visitor }}</span>
          </li>
        </ul>
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { Chart, registerables } from 'chart.js'

Chart.register(...registerables)

const visitors = ref([
  'Alice Dupont',
  'Jean Martin',
  'Sophie Bernard',
  'Marc Leroy',
  'Claire Petit',
  'Paul Durand'
])

const barChart = ref(null)
const donutChart = ref(null)
const lineChart = ref(null)

onMounted(() => {
  // BAR CHART avec dégradé et barres plus élégantes
  const ctxBar = barChart.value.getContext('2d')
  const gradient = ctxBar.createLinearGradient(0, 0, 0, 300)
  gradient.addColorStop(0, '#F97316')
  gradient.addColorStop(1, '#EA580C')
  
  new Chart(barChart.value, {
    type: 'bar',
    data: {
      labels: ['Jan', 'Fév', 'Mar', 'Avr', 'Mai', 'Juin'],
      datasets: [
        {
          label: 'Année 2024',
          data: [20, 30, 40, 25, 50, 35],
          backgroundColor: '#F97316',
          borderRadius: 12,
          barPercentage: 0.65,
          categoryPercentage: 0.8,
        },
        {
          label: 'Année 2025',
          data: [15, 25, 35, 45, 40, 55],
          backgroundColor: '#CBD5E1',
          borderRadius: 12,
          barPercentage: 0.65,
          categoryPercentage: 0.8,
        },
      ],
    },
    options: {
      responsive: true,
      maintainAspectRatio: true,
      plugins: {
        legend: { position: 'bottom', labels: { usePointStyle: true, boxWidth: 8 } },
        tooltip: { backgroundColor: '#1E293B', titleColor: '#F97316', bodyColor: '#fff' }
      },
      scales: {
        x: { grid: { display: false }, ticks: { font: { size: 12 } } },
        y: { grid: { color: '#E2E8F0' }, ticks: { stepSize: 20 } }
      },
      animation: { duration: 1200, easing: 'easeOutQuart' }
    },
  })

  // DONUT CHART avec design moderne
  new Chart(donutChart.value, {
    type: 'doughnut',
    data: {
      labels: ['Satisfait', 'Non satisfait'],
      datasets: [
        {
          data: [45, 55],
          backgroundColor: ['#F97316', '#E2E8F0'],
          borderWidth: 0,
          cutout: '75%',
          borderRadius: 8,
          spacing: 4,
        },
      ],
    },
    options: {
      responsive: true,
      maintainAspectRatio: true,
      plugins: { legend: { display: false }, tooltip: { enabled: true } },
      cutout: '75%',
    },
  })

  // LINE CHART avec dégradé sous la courbe
  const ctxLine = lineChart.value.getContext('2d')
  const gradientLine = ctxLine.createLinearGradient(0, 0, 0, 300)
  gradientLine.addColorStop(0, 'rgba(249, 115, 22, 0.3)')
  gradientLine.addColorStop(1, 'rgba(249, 115, 22, 0.02)')

  new Chart(lineChart.value, {
    type: 'line',
    data: {
      labels: ['Lun', 'Mar', 'Mer', 'Jeu', 'Ven', 'Sam', 'Dim'],
      datasets: [
        {
          label: 'Performance',
          data: [12, 19, 10, 25, 20, 30, 28],
          borderColor: '#F97316',
          backgroundColor: gradientLine,
          fill: true,
          tension: 0.4,
          pointBackgroundColor: '#F97316',
          pointBorderColor: '#fff',
          pointRadius: 4,
          pointHoverRadius: 6,
          borderWidth: 2,
        },
        {
          label: 'Progression',
          data: [8, 14, 9, 20, 15, 25, 22],
          borderColor: '#94A3B8',
          backgroundColor: 'rgba(148, 163, 184, 0.1)',
          fill: true,
          tension: 0.4,
          pointBackgroundColor: '#94A3B8',
          pointBorderColor: '#fff',
          pointRadius: 4,
          pointHoverRadius: 6,
          borderWidth: 2,
          borderDash: [5, 5],
        },
      ],
    },
    options: {
      responsive: true,
      maintainAspectRatio: true,
      plugins: {
        legend: { position: 'bottom', labels: { usePointStyle: true } },
        tooltip: { mode: 'index', intersect: false }
      },
      scales: {
        x: { grid: { display: false } },
        y: { grid: { color: '#E2E8F0' } }
      },
      animation: { duration: 1000, easing: 'easeOutQuart' }
    },
  })
})
</script>

<style scoped>
/* Animation douce pour les cartes */
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(8px); }
  to { opacity: 1; transform: translateY(0); }
}
* {
  transition: all 0.2s ease;
}
</style>