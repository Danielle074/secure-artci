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

// Interface pour les données du formulaire (sans id)
interface NewCompanyData {
  nom: string
  logo: string
  phone?: string
  email?: string
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

// ✅ FONCTION D'AJOUT CORRIGÉE
function addCompany(newCompany: NewCompanyData) {
  const nextId = companies.value.length + 1
  const completeCompany: Company = {
    id: nextId,
    nom: newCompany.nom,
    logo: newCompany.logo || 'https://via.placeholder.com/80?text=Logo',
    qrCode: `https://api.qrserver.com/v1/create-qr-code/?size=80x80&data=${encodeURIComponent(newCompany.nom)}`,
    lien: '#',
    phone: newCompany.phone || '',
    email: newCompany.email || '',
    members: []
  }
  companies.value.push(completeCompany)
  goToList()
}
</script>