<template>
  <header class="h-24 flex items-center justify-between px-6 bg-white shadow-md">
    <!-- Titre à gauche -->
    <h1 class="text-2xl font-semibold text-gray-800">{{ pageTitle }}</h1>

    <!-- Barre de recherche + avatar + boutons -->
    <div class="flex items-center space-x-6">
      <input
        type="text"
        placeholder="Recherche..."
        class="px-3 py-2 w-64 border rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-600"
      />

      <!-- Info utilisateur cliquable -->
      <div class="relative">
        <div class="flex items-center space-x-3 cursor-pointer" @click="toggleDropdown">
          <img :src="avatar" alt="Avatar" class="h-12 w-12 rounded-full object-cover" />
          <div class="flex flex-col">
            <span class="font-semibold text-gray-800">Jane</span>
            <span class="text-sm text-gray-500">utilisateur</span>
          </div>
          <i class="bx bx-chevron-down text-gray-500"></i>
        </div>

        <!-- Dropdown amélioré -->
        <transition name="fade">
          <div
            v-if="isDropdownOpen"
            class="absolute right-0 mt-2 w-56 bg-white rounded-xl shadow-xl overflow-hidden z-50 border border-gray-200"
          >
            <div class="px-4 py-3 bg-[#f9fafb] flex items-center space-x-3">
              <img :src="avatar" alt="Avatar" class="h-10 w-10 rounded-full object-cover border border-gray-300" />
              <div class="flex flex-col">
                <p class="font-semibold text-gray-800">Jane Doe</p>
                <p class="text-sm text-gray-500">utilisateur</p>
              </div>
            </div>

            <div class="px-4 py-2 flex items-center justify-between border-b border-gray-200">
              <span class="text-gray-600 font-medium">Statut :</span>
              <span class="text-sm font-semibold">
                <span v-if="status === 'online'" class="text-green-500">En ligne</span>
                <span v-else-if="status === 'busy'" class="text-yellow-500">Indisponible</span>
                <span v-else class="text-gray-400">Hors ligne</span>
              </span>
            </div>

            <div class="px-4 py-3 flex justify-center">
              <button
                @click="logout"
                class="w-2/3 text-white bg-red-600 hover:bg-red-700 transition px-4 py-2 rounded-lg font-semibold shadow-md"
              >
                Déconnexion
              </button>
            </div>
          </div>
        </transition>
      </div>

      <!-- Boutons -->
      <button class="p-2 rounded-lg hover:bg-gray-200 transition">
        <i class='bx bx-cog text-xl text-gray-600'></i>
      </button>
      <button class="p-2 rounded-lg hover:bg-gray-200 transition relative">
        <i class='bx bx-bell text-xl text-gray-600'></i>
        <span class="absolute top-0 right-0 h-2 w-2 bg-red-500 rounded-full border border-white"></span>
      </button>
    </div>
  </header>
</template>

<script setup lang="ts">
import avatarImage from '@/assets/images/photologin.jpg'
import { ref } from 'vue'

// Définition des props (defineProps est automatique en script setup)
const props = defineProps<{
  pageTitle?: string
}>()

const avatar = avatarImage
const isDropdownOpen = ref(false)
const status = ref<'online' | 'busy' | 'offline'>('online')

const toggleDropdown = () => {
  isDropdownOpen.value = !isDropdownOpen.value
}

const logout = () => {
  console.log("Déconnexion effectuée")
  // À compléter avec la vraie logique de déconnexion
}
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: all 0.2s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(-5px);
}
</style>