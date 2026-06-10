<template>
  <!-- Overlay pour mobile -->
  <div 
    v-if="isMobileMenuOpen" 
    @click="closeMobileMenu"
    class="fixed inset-0 bg-black/50 z-40 lg:hidden transition-opacity duration-300"
    :class="isMobileMenuOpen ? 'opacity-100' : 'opacity-0 pointer-events-none'"
  ></div>

  <!-- Sidebar -->
  <aside
    :class="[
      'fixed top-0 left-0 z-50 h-screen flex flex-col bg-gray-100 border-r border-gray-200 transition-all duration-300 ease-in-out',
      isMobileMenuOpen ? 'translate-x-0' : '-translate-x-full lg:translate-x-0',
      'w-72 min-w-[18rem] max-w-[18rem] lg:w-64 lg:min-w-[16rem] lg:max-w-[16rem]'
    ]"
  >
    <!-- Logo -->
    <div class="flex items-center justify-between h-24 bg-white shadow-sm rounded-b-lg shrink-0 px-4">
      <a href="/" class="flex items-center justify-center flex-1">
        <img :src="logo" alt="Logo" class="h-16 w-16 cursor-pointer" />
      </a>
      <!-- Bouton fermeture mobile -->
      <button 
        @click="closeMobileMenu"
        class="lg:hidden text-gray-500 hover:text-orange-500 transition p-2"
      >
        <i class="bx bx-x text-2xl"></i>
      </button>
    </div>

    <!-- MENU SCROLLABLE -->
    <div class="flex-1 overflow-y-auto scrollbar-hidden px-4 py-6">
      <ul class="space-y-1 text-sm font-medium">
        <!-- Utilisateurs -->
        <li>
          <router-link
            to="/dashboard/utilisateurs/list"
            @click="closeMobileMenuOnLink"
            class="flex items-center gap-3 px-4 py-3 rounded-lg hover:bg-orange-50 text-gray-700 hover:text-orange-600 transition-all duration-200"
            active-class="bg-orange-50 text-orange-600"
          >
            <i class="bx bx-user text-xl"></i>
            <span class="flex-1">Utilisateurs</span>
            <i class="bx bxs-chevron-right text-sm text-gray-400"></i>
          </router-link>
        </li>

        <!-- Scanners QR Code -->
        <li>
          <router-link
            to="/dashboard/scanner"
            @click="closeMobileMenuOnLink"
            class="flex items-center gap-3 px-4 py-3 rounded-lg hover:bg-orange-50 text-gray-700 hover:text-orange-600 transition-all duration-200"
            active-class="bg-orange-50 text-orange-600"
          >
            <i class="bx bx-qr-scan text-xl"></i>
            <span class="flex-1">Scanners QR Code</span>
            <i class="bx bxs-chevron-right text-sm text-gray-400"></i>
          </router-link>
        </li>

        <!-- Évènements -->
        <li>
          <router-link
            to="/dashboard/evenement"
            @click="closeMobileMenuOnLink"
            class="flex items-center gap-3 px-4 py-3 rounded-lg hover:bg-orange-50 text-gray-700 hover:text-orange-600 transition-all duration-200"
            active-class="bg-orange-50 text-orange-600"
          >
            <i class="bx bx-calendar-event text-xl"></i>
            <span class="flex-1">Évènements</span>
            <i class="bx bxs-chevron-right text-sm text-gray-400"></i>
          </router-link>
        </li>

        <!-- Entreprises -->
        <li>
          <router-link
            to="/dashboard/entreprises"
            @click="closeMobileMenuOnLink"
            class="flex items-center gap-3 px-4 py-3 rounded-lg hover:bg-orange-50 text-gray-700 hover:text-orange-600 transition-all duration-200"
            active-class="bg-orange-50 text-orange-600"
          >
            <i class="bx bx-building text-xl"></i>
            <span class="flex-1">Entreprises</span>
            <i class="bx bxs-chevron-right text-sm text-gray-400"></i>
          </router-link>
        </li>

        <!-- Liste des visiteurs -->
        <li>
          <router-link
            to="/dashboard/visiteurs"
            @click="closeMobileMenuOnLink"
            class="flex items-center gap-3 px-4 py-3 rounded-lg hover:bg-orange-50 text-gray-700 hover:text-orange-600 transition-all duration-200"
            active-class="bg-orange-50 text-orange-600"
          >
            <i class="bx bx-list-ul text-xl"></i>
            <span class="flex-1">Liste des visiteurs</span>
            <i class="bx bxs-chevron-right text-sm text-gray-400"></i>
          </router-link>
        </li>

        <!-- Demande -->
        <li>
          <router-link
            to="/dashboard/demande"
            @click="closeMobileMenuOnLink"
            class="flex items-center gap-3 px-4 py-3 rounded-lg hover:bg-orange-50 text-gray-700 hover:text-orange-600 transition-all duration-200"
            active-class="bg-orange-50 text-orange-600"
          >
            <i class="bx bx-envelope text-xl"></i>
            <span class="flex-1">Demandes</span>
            <i class="bx bxs-chevron-right text-sm text-gray-400"></i>
          </router-link>
        </li>

        <!-- Liste de présence -->
        <li>
          <router-link
            to="/dashboard/presence"
            @click="closeMobileMenuOnLink"
            class="flex items-center gap-3 px-4 py-3 rounded-lg hover:bg-orange-50 text-gray-700 hover:text-orange-600 transition-all duration-200"
            active-class="bg-orange-50 text-orange-600"
          >
            <i class="bx bx-check-square text-xl"></i>
            <span class="flex-1">Liste de présence</span>
            <i class="bx bxs-chevron-right text-sm text-gray-400"></i>
          </router-link>
        </li>

        <!-- Carte de membre -->
        <li>
          <router-link
            to="/dashboard/carte"
            @click="closeMobileMenuOnLink"
            class="flex items-center gap-3 px-4 py-3 rounded-lg hover:bg-orange-50 text-gray-700 hover:text-orange-600 transition-all duration-200"
            active-class="bg-orange-50 text-orange-600"
          >
            <i class="bx bx-id-card text-xl"></i>
            <span class="flex-1">Carte de membre</span>
            <i class="bx bxs-chevron-right text-sm text-gray-400"></i>
          </router-link>
        </li>

        <!-- Liste de courriers -->
        <li>
          <router-link
            to="/dashboard/courriers/liste"
            @click="closeMobileMenuOnLink"
            class="flex items-center gap-3 px-4 py-3 rounded-lg hover:bg-orange-50 text-gray-700 hover:text-orange-600 transition-all duration-200"
            active-class="bg-orange-50 text-orange-600"
          >
            <i class="bx bx-mail-send text-xl"></i>
            <span class="flex-1">Liste de courriers</span>
            <i class="bx bxs-chevron-right text-sm text-gray-400"></i>
          </router-link>
        </li>

        <!-- Tâches -->
        <li>
          <router-link
            to="/dashboard/taches/liste"
            @click="closeMobileMenuOnLink"
            class="flex items-center gap-3 px-4 py-3 rounded-lg hover:bg-orange-50 text-gray-700 hover:text-orange-600 transition-all duration-200"
            active-class="bg-orange-50 text-orange-600"
          >
            <i class="bx bx-task text-xl"></i>
            <span class="flex-1">Tâches</span>
            <i class="bx bxs-chevron-right text-sm text-gray-400"></i>
          </router-link>
        </li>

        <!-- Projets -->
        <li>
          <router-link
            to="/dashboard/projet/liste"
            @click="closeMobileMenuOnLink"
            class="flex items-center gap-3 px-4 py-3 rounded-lg hover:bg-orange-50 text-gray-700 hover:text-orange-600 transition-all duration-200"
            active-class="bg-orange-50 text-orange-600"
          >
            <i class="bx bx-folder text-xl"></i>
            <span class="flex-1">Projets</span>
            <i class="bx bxs-chevron-right text-sm text-gray-400"></i>
          </router-link>
        </li>

        <!-- Documents -->
        <li>
          <router-link
            to="/dashboard/document"
            @click="closeMobileMenuOnLink"
            class="flex items-center gap-3 px-4 py-3 rounded-lg hover:bg-orange-50 text-gray-700 hover:text-orange-600 transition-all duration-200"
            active-class="bg-orange-50 text-orange-600"
          >
            <i class="bx bx-file text-xl"></i>
            <span class="flex-1">Documents</span>
            <i class="bx bxs-chevron-right text-sm text-gray-400"></i>
          </router-link>
        </li>
      </ul>
    </div>

    <!-- PROFIL EN BAS AVEC AVATAR -->
    <div class="px-4 py-6 bg-white border-t border-gray-200 mt-auto space-y-3 shrink-0">
      <!-- Section Avatar et informations -->
      <div class="flex items-center gap-3">
        <div class="relative group">
          <img 
            :src="avatar" 
            alt="Avatar" 
            class="h-12 w-12 rounded-full border-2 border-orange-500 object-cover cursor-pointer hover:opacity-90 transition"
          />
          <!-- Badge en ligne -->
          <div class="absolute bottom-0 right-0 h-3 w-3 bg-green-500 rounded-full border-2 border-white"></div>
        </div>
        <div class="flex-1 min-w-0">
          <h4 class="font-semibold text-gray-800 truncate">{{ userData.name }}</h4>
          <p class="text-xs text-gray-500 truncate">{{ userData.role }}</p>
        </div>
        <button 
          @click="editProfile" 
          class="text-gray-400 hover:text-orange-500 transition shrink-0"
          title="Modifier le profil"
        >
          <i class="bx bx-pencil text-sm"></i>
        </button>
      </div>

      <!-- Statistiques rapides -->
      <div class="grid grid-cols-2 gap-2 text-center">
        <div class="bg-gray-50 rounded-lg p-2">
          <p class="text-xs text-gray-500">Projets</p>
          <p class="text-sm font-semibold text-gray-800">{{ userData.projects }}</p>
        </div>
        <div class="bg-gray-50 rounded-lg p-2">
          <p class="text-xs text-gray-500">Tâches</p>
          <p class="text-sm font-semibold text-gray-800">{{ userData.tasks }}</p>
        </div>
      </div>

      <!-- Boutons Profil et Déconnexion -->
      <div class="flex gap-2">
        <router-link
          to="/dashboard/profile"
          @click="closeMobileMenuOnLink"
          class="flex-1 text-center bg-gray-200 hover:bg-gray-300 text-gray-800 py-2 rounded-lg text-sm font-medium transition"
        >
          <i class="bx bx-user-circle mr-1"></i>
          Profil
        </router-link>
        <button
          @click="logout"
          class="flex-1 bg-orange-500 hover:bg-orange-600 text-white py-2 rounded-lg text-sm font-semibold transition"
        >
          <i class="bx bx-log-out mr-1"></i>
          Déconnexion
        </button>
      </div>
    </div>
  </aside>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import { useRouter } from 'vue-router'
import logoImage from '@/assets/images/logo-artci.png'
import avatarImage from '@/assets/images/identite.jpg'

const router = useRouter()
const logo = logoImage
const avatar = avatarImage
const isMobileMenuOpen = ref(false)

// Données utilisateur
const userData = ref({
  name: 'Jean Dupont',
  role: 'Administrateur',
  projects: 12,
  tasks: 45
})

// Gestion de la déconnexion
const logout = () => {
  localStorage.removeItem('token')
  localStorage.removeItem('user')
  sessionStorage.clear()
  router.push('/login')
}

// Modification du profil
const editProfile = () => {
  closeMobileMenuOnLink()
  router.push('/dashboard/profile/edit')
}

// Fermer le menu mobile
const closeMobileMenu = () => {
  isMobileMenuOpen.value = false
}

const closeMobileMenuOnLink = () => {
  if (window.innerWidth < 1024) {
    isMobileMenuOpen.value = false
  }
}

// Ouvrir le menu mobile (sera appelé depuis le Header)
const openMobileMenu = () => {
  isMobileMenuOpen.value = true
}

// Exposer la méthode pour le Header
defineExpose({ openMobileMenu })

// Chargement des données utilisateur
onMounted(() => {
  const user = localStorage.getItem('user')
  if (user) {
    const userDataObj = JSON.parse(user)
    userData.value.name = userDataObj.name || userData.value.name
    userData.value.role = userDataObj.role || userData.value.role
  }
})
</script>

<style scoped>
.scrollbar-hidden {
  scrollbar-width: none;
  -ms-overflow-style: none;
}

.scrollbar-hidden::-webkit-scrollbar {
  display: none;
}

.router-link-active {
  position: relative;
}

.router-link-active::before {
  content: '';
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  height: 60%;
  width: 4px;
  background-color: #f97316;
  border-radius: 0 4px 4px 0;
}

.router-link-active i:last-child {
  transform: rotate(90deg);
  transition: transform 0.2s ease;
  color: #f97316;
}

li a i:last-child {
  transition: transform 0.2s ease;
}

/* Animation pour l'avatar au survol */
.group:hover img {
  transform: scale(1.05);
  transition: transform 0.2s ease;
}

/* Responsive adjustments */
@media (max-width: 640px) {
  .w-72 {
    width: 85%;
    max-width: 280px;
  }
}
</style>