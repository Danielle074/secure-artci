<template>
  <aside
    class="fixed top-0 left-0 z-40
           w-64 min-w-[16rem] max-w-[16rem]
           h-screen flex flex-col
           bg-white shadow-xl
           border-r border-gray-100
           rounded-tr-2xl rounded-br-2xl
           overflow-hidden transition-all duration-300"
  >
    <!-- Logo avec ombre légère -->
    <div class="flex items-center justify-center h-24 bg-gradient-to-r from-white to-orange-50/30 shadow-sm rounded-b-2xl shrink-0 border-b border-orange-100">
      <router-link to="/" class="flex items-center justify-center group">
        <img :src="logo" alt="Logo ARTCI" class="h-12 w-12 " />
      
      </router-link>
    </div>

    <!-- MENU SCROLLABLE avec espacement amélioré -->
    <div class="flex-1 overflow-y-auto scrollbar-hidden px-3 py-6">
      <ul class="space-y-1.5 text-sm font-medium">
        <!-- Utilisateurs -->
        <li v-for="item in menuItems" :key="item.to">
          <router-link
            :to="item.to"
            class="flex items-center gap-3 px-4 py-2.5 rounded-xl hover:bg-orange-50 text-gray-700 hover:text-orange-600 transition-all duration-200 group"
            active-class="bg-orange-50 text-orange-600 shadow-sm"
          >
            <i :class="[item.icon, 'text-xl transition-transform group-hover:scale-105']"></i>
            <span class="flex-1">{{ item.label }}</span>
            <i class="bx bx-chevron-right text-sm text-gray-400 transition-transform duration-200 group-hover:translate-x-0.5"></i>
          </router-link>
        </li>
      </ul>
    </div>

    <!-- PROFIL EN BAS -->
    <div class="px-4 py-5 bg-gray-50/80 border-t border-gray-100 mt-auto space-y-4 shrink-0 backdrop-blur-sm">
      <div class="flex items-center gap-3">
        <div class="relative">
          <img :src="logo" alt="Avatar" class="h-10 w-10 rounded-full border-2 border-orange-300 object-cover shadow-sm" />
          <span class="absolute bottom-0 right-0 w-3 h-3 bg-green-500 rounded-full border-2 border-white"></span>
        </div>
        <div>
          <h4 class="font-semibold text-gray-800 text-sm">Admin ARTCI</h4>
          <p class="text-xs text-gray-500">Superviseur</p>
        </div>
      </div>

      <div class="flex gap-2">
        <router-link
          to="/dashboard/profile"
          class="flex-1 text-center bg-white hover:bg-gray-100 text-gray-700 py-2 rounded-xl text-sm font-medium transition shadow-sm"
        >
          <i class="bx bx-user-circle mr-1"></i> Profil
        </router-link>
        <button
          @click="logout"
          class="flex-1 bg-orange-500 hover:bg-orange-600 text-white py-2 rounded-xl text-sm font-semibold transition shadow-md flex items-center justify-center gap-1"
        >
          <i class="bx bx-log-out"></i> Quitter
        </button>
      </div>
    </div>
  </aside>
</template>

<script setup lang="ts">
import logoImage from '@/assets/images/logoartci.png'

const logo = logoImage

// Menu items centralisé pour plus de maintenabilité
const menuItems = [
  { to: '/dashboard/utilisateurs/list', label: 'Utilisateurs', icon: 'bx bx-user' },
  { to: '/dashboard/scanner', label: 'Scanners QR Code', icon: 'bx bx-qr-scan' },
  { to: '/dashboard/evenement', label: 'Évènements', icon: 'bx bx-calendar-event' },
  { to: '/dashboard/entreprises', label: 'Entreprises', icon: 'bx bx-building' },
  { to: '/dashboard/visiteurs', label: 'Liste des visiteurs', icon: 'bx bx-list-ul' },
  { to: '/dashboard/demande', label: 'Demandes', icon: 'bx bx-envelope' },
  { to: '/dashboard/presence', label: 'Liste de présence', icon: 'bx bx-check-square' },
  { to: '/dashboard/carte', label: 'Carte de membre', icon: 'bx bx-id-card' },
  { to: '/dashboard/courriers/liste', label: 'Liste de courriers', icon: 'bx bx-mail-send' },
  { to: '/dashboard/taches/liste', label: 'Tâches', icon: 'bx bx-task' },
  { to: '/dashboard/projet/liste', label: 'Projets', icon: 'bx bx-folder' },
  { to: '/dashboard/document', label: 'Documents', icon: 'bx bx-file' }
]

const logout = () => {
  // Tu peux aussi faire un appel API de déconnexion
  window.location.href = '/login'
}
</script>

<style scoped>
/* Cache la scrollbar mais garde le scroll */
.scrollbar-hidden {
  scrollbar-width: thin;
  scrollbar-color: #f97316 #f1f1f1;
}
.scrollbar-hidden::-webkit-scrollbar {
  width: 4px;
}
.scrollbar-hidden::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 10px;
}
.scrollbar-hidden::-webkit-scrollbar-thumb {
  background: #f97316;
  border-radius: 10px;
}

/* Indicateur actif plus élégant */
.router-link-active {
  position: relative;
  font-weight: 500;
}
.router-link-active i:last-child {
  transform: translateX(2px);
  color: #f97316;
}
</style>