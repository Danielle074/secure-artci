<template>
  <div class="p-6">
    <!-- Bouton retour (quand on est en création) -->
    <div v-if="activeTab === 'create'" class="mb-4">
      <button
        @click="goToList"
        class="flex items-center text-orange-600 hover:text-orange-700 font-medium"
      >
        <i class="bx bx-arrow-back text-xl"></i>
        <span class="ml-1">Retour</span>
      </button>
    </div>

    <!-- En-tête -->
    <div class="flex justify-between items-center mb-6">
      <!-- Bouton créer -->
      <button
        v-if="activeTab === 'list'"
        @click="goToCreate"
        class="px-4 py-2 rounded-lg font-semibold bg-orange-600 text-white hover:bg-orange-700 transition"
      >
        Créer une Entreprise
      </button>
    </div>

    <!-- Création -->
    <AddCompany
      v-if="activeTab === 'create'"
      @add-company="addCompany"
    />

    <!-- Liste -->
    <CompanyList
      v-else
      :companies="companies"
      @open-badge="openBadge"
      @open-members="openMembers"
      @open-edit="openEdit"
      @delete-company="deleteCompany"
      @copy-link="copyToClipboard"
    />

    <!-- Modals -->
    <BadgeModal
      v-if="showBadge"
      :company="selectedCompany"
      @close="showBadge = false"
    />

    <MembersModal
      v-if="showMembersModal"
      :company="selectedCompany"
      @close="showMembersModal = false"
    />

    <EditCompanyForm
      v-if="showEditForm"
      :company="selectedCompany"
      @close="showEditForm = false"
      @submit="updateCompany"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import { useRoute, useRouter } from 'vue-router'

/* IMPORTS */
import AddCompany from '@/components/dashboard/AddCompany.vue'
import CompanyList from '@/components/dashboard/CompanyList.vue'
import BadgeModal from '@/components/dashboard/BadgeModal.vue'
import MembersModal from '@/components/dashboard/MembersModal.vue'
import EditCompanyForm from '@/components/dashboard/EditCompanyForm.vue'

const route = useRoute()
const router = useRouter()

const showBadge = ref(false)
const showMembersModal = ref(false)
const showEditForm = ref(false)
const selectedCompany = ref<any>(null)

interface Company {
  id: number
  nom: string
  logo: string
  qrCode: string
  lien: string
  phone?: string
  email?: string
  members?: string[]
}

/* Données mock */
const companies = ref<Company[]>([])
for (let i = 1; i <= 5; i++) {
  companies.value.push({
    id: i,
    nom: `Entreprise ${i}`,
    logo: `https://via.placeholder.com/80?text=Logo+${i}`,
    qrCode: `https://api.qrserver.com/v1/create-qr-code/?size=80x80&data=Entreprise+${i}`,
    lien: '#',
    phone: '0123456789',
    email: `entreprise${i}@mail.com`,
    members: [`Membre ${i}-A`, `Membre ${i}-B`],
  })
}

/* TAB basé sur la route */
const activeTab = computed(() => {
  return route.name === 'addCompany' ? 'create' : 'list'
})

/* Navigation */
function goToCreate() {
  router.push({ name: 'addCompany' })
}

function goToList() {
  router.push({ name: 'companyList' })
}

/* Modals */
function openBadge(company: Company) {
  selectedCompany.value = company
  showBadge.value = true
}

function openMembers(company: Company) {
  selectedCompany.value = company
  showMembersModal.value = true
}

function openEdit(company: Company) {
  selectedCompany.value = { ...company }
  showEditForm.value = true
}

function deleteCompany(index: number) {
  if (confirm('Voulez-vous supprimer cette entreprise ?')) {
    companies.value.splice(index, 1)
  }
}

function updateCompany(updated: Company) {
  const idx = companies.value.findIndex(c => c.id === updated.id)
  if (idx !== -1) companies.value[idx] = updated
  showEditForm.value = false
}

function copyToClipboard(text: string) {
  navigator.clipboard.writeText(text)
  alert('Lien copié !')
}

function addCompany(company: Company) {
  const nextId = companies.value.length + 1
  companies.value.push({ ...company, id: nextId })
  goToList()
}
</script>
