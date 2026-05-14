<template>
  <div class="flex h-screen bg-gray-100 overflow-hidden">
    <!-- Sidebar (fixe) -->
    <Sidebar />

    <!-- Zone principale décalée de la sidebar -->
    <div class="ml-64 flex-1 flex flex-col overflow-hidden">
      
      <!-- Header -->
      <Header :pageTitle="pageTitle" />

      <!-- Contenu dynamique -->
      <main class="flex-1 p-6 overflow-y-auto overflow-x-hidden hide-scrollbar">
        <router-view />
      </main>

    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'
import { useRoute } from 'vue-router'
import Sidebar from '@/components/Sidebar.vue'
import Header from '@/components/Header.vue'

const route = useRoute()

const pageTitle = computed(() => {
  switch (route.name) {

    // 🏠 DASHBOARD
    case 'dashboard':
      return 'Accueil'

    // 👥 UTILISATEURS
    case 'userList':
      return 'Utilisateurs > Liste'
    case 'addUser':
      return 'Utilisateurs > Ajouter'

    // 📩 DEMANDES
    case 'demande':
      return 'Demandes'
    case 'DetailsDemande':
      return 'Demandes > Détails'

    // 📷 SCANNER
    case 'listeQRCode':
      return 'Scanner > Liste QR Code'
    case 'scannerQRCode':
      return 'Scanner > QR Code'
    case 'qrManuel':
      return 'Scanner > Manuel'
    case 'creerQRCode':
      return 'Scanner > Créer QR Code'

    // 🎉 ÉVÉNEMENTS
    case 'listeEvenement':
      return 'Évènements > Liste'
    case 'ajouterEvenement':
      return 'Évènements > Ajouter'

    // 🏢 ENTREPRISES
    case 'companyList':
      return 'Entreprises > Liste'
    case 'addCompany':
      return 'Entreprises > Ajouter'
    case 'editCompany':
      return 'Entreprises > Modifier'
    case 'badgeCompany':
      return 'Entreprises > Badge'
    case 'membersCompany':
      return 'Entreprises > Membres'

    // 💳 CARTES
    case 'cardList':
      return 'Cartes > Liste'
    case 'createCard':
      return 'Cartes > Créer'

    // 📩 COURRIERS
    case 'ListeCourrier':
      return 'Courriers > Liste'
    case 'ajouterCourrier':
      return 'Courriers > Ajouter'

    // 📋 PRÉSENCE
    case 'presence':
      return 'Liste de présence'

    default:
      return 'Tableau de bord'
  }
})
</script>

<style scoped>
/* Cache la scrollbar mais garde le scroll */
.hide-scrollbar::-webkit-scrollbar {
  display: none;
}

.hide-scrollbar {
  -ms-overflow-style: none;
  scrollbar-width: none;
}
</style>
