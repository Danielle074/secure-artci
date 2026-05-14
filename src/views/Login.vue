<template>
  <div class="min-h-screen flex flex-col md:flex-row">
    <!-- Formulaire à gauche (centré verticalement) -->
    <div class="flex w-full md:w-1/2 items-center justify-center bg-gray-50">
      <div class="p-8 w-full max-w-md">
        <!-- Logo ARTCI -->
        <div class="flex justify-center mb-4">
          <img :src="artciLogo" alt="ARTCI Logo" class="h-16 w-auto object-contain" />
        </div>

        <form @submit.prevent="handleLogin" class="space-y-6">
          <!-- Email -->
          <div>
            <input
              v-model="email"
              type="email"
              placeholder="Email professionnel"
              class="w-full px-4 py-3 border border-gray-300 rounded-xl focus:outline-none focus:ring-2 focus:ring-orange-500 focus:border-orange-500 transition"
              required
            />
          </div>

          <!-- Mot de passe avec œil -->
          <div class="relative">
            <input
              v-model="password"
              :type="showPassword ? 'text' : 'password'"
              placeholder="Mot de passe"
              class="w-full px-4 py-3 border border-gray-300 rounded-xl focus:outline-none focus:ring-2 focus:ring-orange-500 focus:border-orange-500 transition pr-12"
              required
            />
            <button
              type="button"
              @click="showPassword = !showPassword"
              class="absolute inset-y-0 right-0 flex items-center pr-3 text-gray-500 hover:text-orange-600"
            >
              <i :class="showPassword ? 'bx bx-show' : 'bx bx-hide'"></i>
            </button>
          </div>

          <!-- Options -->
          <div class="flex items-center justify-between text-sm">
            <label class="flex items-center space-x-2 text-gray-600">
              <input type="checkbox" v-model="rememberMe" class="rounded border-gray-300 text-orange-600 focus:ring-orange-500" />
              <span>Rester connecté</span>
            </label>
            <a href="#" class="text-orange-600 hover:underline">Mot de passe oublié ?</a>
          </div>

          <!-- Bouton Connexion (GROS) -->
          <button
            type="submit"
            class="w-full bg-orange-600 hover:bg-orange-700 text-white font-bold text-lg py-4 rounded-xl transition duration-200 transform hover:scale-[1.02] shadow-md"
          >
            Se connecter
          </button>
        </form>

        <!-- Lien création compte -->
        <p class="text-center text-gray-600 text-sm mt-6">
          Vous n'avez pas de compte ?
          <a href="#" class="text-orange-600 font-semibold hover:underline">Créer un compte</a>
        </p>

        <p class="text-center text-gray-400 text-xs mt-8">
          © 2025 ARTCI – Côte d'Ivoire
        </p>
      </div>
    </div>

    <!-- Image à droite - prend toute la hauteur/largeur de sa moitié avec overlay noir -->
    <div class="hidden md:flex md:w-1/2 relative bg-cover bg-center bg-no-repeat"
         :style="{ backgroundImage: `url(${loginImage})` }">
      <!-- Overlay noir transparent -->
      <div class="absolute inset-0 bg-black/50"></div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import artciLogo from '@/assets/images/logoartci.png'
import loginImage from '@/assets/images/photologin.jpg'

const router = useRouter()
const email = ref('')
const password = ref('')
const rememberMe = ref(false)
const showPassword = ref(false)

const handleLogin = () => {
  // Ici, vous effectuez votre appel API d'authentification
  console.log('Connexion ARTCI', { email: email.value, password: password.value, rememberMe: rememberMe.value })
  
  // Exemple : si authentification réussie, rediriger vers le dashboard
  // À remplacer par votre vraie logique
  if (email.value && password.value) {
    router.push('/dashboard')
  } else {
    alert('Veuillez saisir un email et un mot de passe')
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');
* {
  font-family: 'Inter', sans-serif;
}
</style>