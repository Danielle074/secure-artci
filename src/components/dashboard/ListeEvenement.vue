<template>
  <div class="bg-white shadow-xl rounded-2xl p-6">
    <!-- Vue Liste des événements -->
    <div v-if="activeTab === 'list'">
      <div class="flex justify-between items-center mb-6">
        <h3 class="text-xl font-bold text-orange-600">📅 Événements</h3>
        <button class="bg-orange-500 hover:bg-orange-600 text-white px-4 py-2 rounded-xl text-sm font-semibold shadow transition">
          + Nouvel événement
        </button>
      </div>

      <!-- Tableau responsive -->
      <div class="overflow-x-auto">
        <table class="min-w-full border border-gray-200 rounded-xl">
          <thead class="bg-orange-600 text-white rounded-t-xl">
            <tr>
              <th class="p-3 text-left rounded-tl-xl"><input type="checkbox" class="rounded" /></th>
              <th class="p-3 text-left">Nom</th>
              <th class="p-3 text-left">Entreprise</th>
              <th class="p-3 text-left">Lieu</th>
              <th class="p-3 text-left">Date</th>
              <th class="p-3 text-left">Heure</th>
              <th class="p-3 text-left">Création</th>
              <th class="p-3 text-left">Lien</th>
              <th class="p-3 text-left">Participation</th>
              <th class="p-3 text-left">QR Code</th>
              <th class="p-3 text-left rounded-tr-xl">Action</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(event, idx) in paginatedEvents" :key="event.id" class="border-b hover:bg-orange-50 transition">
              <td class="p-3"><input type="checkbox" v-model="event.selected" /></td>
              <td class="p-3 font-medium text-blue-700 hover:underline cursor-pointer" @click="voirParticipants(event)">
                {{ event.nom }}
              </td>
              <td class="p-3">{{ event.entreprise }}</td>
              <td class="p-3">{{ event.lieu }}</td>
              <td class="p-3">{{ event.date }}</td>
              <td class="p-3">{{ event.heure }}</td>
              <td class="p-3">{{ event.createdAt }}</td>
              <td class="p-3"><a :href="event.lien" target="_blank" class="text-orange-500 hover:underline">Voir</a></td>
              <td class="p-3">
                <button class="bg-orange-100 text-orange-700 px-2 py-1 rounded-full text-xs font-semibold" @click="voirParticipants(event)">
                  {{ event.participation }} participants
                </button>
              </td>
              <td class="p-3"><img :src="event.qrCode" class="h-10 w-10 rounded shadow-sm" /></td>
              <td class="p-3 relative">
                <button @click="event.dropdown = !event.dropdown" class="p-1 rounded-full hover:bg-gray-100">
                  <i class='bx bx-dots-vertical-rounded text-xl text-gray-600'></i>
                </button>
                <div v-if="event.dropdown" class="absolute right-0 mt-2 w-36 bg-white border rounded-xl shadow-lg z-20">
                  <ul class="text-sm">
                    <li class="px-4 py-2 hover:bg-gray-100 rounded-t-xl cursor-pointer flex items-center gap-2" @click="ajouterEvent(event)"><i class='bx bx-plus-circle'></i> Ajouter</li>
                    <li class="px-4 py-2 hover:bg-gray-100 cursor-pointer flex items-center gap-2" @click="modifierEvent(event)"><i class='bx bx-edit'></i> Modifier</li>
                    <li class="px-4 py-2 hover:bg-red-50 text-red-600 rounded-b-xl cursor-pointer flex items-center gap-2" @click="supprimerEvent(event.id)"><i class='bx bx-trash'></i> Supprimer</li>
                  </ul>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <!-- Pagination -->
      <div class="flex flex-wrap justify-between items-center mt-6 gap-3">
        <div class="text-sm text-gray-500">
          Afficher 
          <select v-model="perPage" class="border rounded-lg p-1 bg-white">
            <option v-for="n in [5,10,15]" :key="n" :value="n">{{ n }}</option>
          </select>
          éléments
        </div>
        <div class="flex gap-1">
          <button :disabled="currentPage === 1" @click="currentPage--" class="px-3 py-1 border rounded-lg hover:bg-gray-100 disabled:opacity-50">‹</button>
          <button v-for="page in totalPages" :key="page" @click="currentPage = page"
                  :class="['px-3 py-1 border rounded-lg hover:bg-gray-100', currentPage === page ? 'bg-orange-600 text-white border-orange-600' : '']">
            {{ page }}
          </button>
          <button :disabled="currentPage === totalPages" @click="currentPage++" class="px-3 py-1 border rounded-lg hover:bg-gray-100 disabled:opacity-50">›</button>
        </div>
      </div>
    </div>

    <!-- Vue Participants -->
    <div v-if="activeTab === 'participants'">
      <div class="flex justify-between items-center mb-6">
        <h3 class="text-xl font-bold text-orange-600">
          👥 Participants – {{ selectedEvent?.nom }}
        </h3>
        <button @click="activeTab = 'list'" class="bg-gray-200 hover:bg-gray-300 text-gray-800 px-5 py-2 rounded-xl transition flex items-center gap-2">
          <i class='bx bx-arrow-back'></i> Retour
        </button>
      </div>

      <div class="overflow-x-auto">
        <table class="min-w-full border border-gray-200 rounded-xl">
          <thead class="bg-orange-600 text-white">
            <tr>
              <th class="p-3 text-left">ID</th>
              <th class="p-3 text-left">Email</th>
              <th class="p-3 text-left">Photo</th>
              <th class="p-3 text-left">Adresse</th>
              <th class="p-3 text-left">Date d'inscription</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="visitor in selectedEvent?.visitors ?? []" :key="visitor.id" class="border-b hover:bg-gray-50">
              <td class="p-3 font-mono text-sm">{{ visitor.id }}</td>
              <td class="p-3">{{ visitor.email }}</td>
              <td class="p-3"><img :src="visitor.image" class="h-10 w-10 rounded-full object-cover shadow" /></td>
              <td class="p-3">{{ visitor.address }}</td>
              <td class="p-3">{{ visitor.createdAt }}</td>
            </tr>
            <tr v-if="!selectedEvent?.visitors?.length">
              <td colspan="5" class="text-center p-6 text-gray-400">Aucun participant pour cet événement.</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

// ================== TYPES ==================
interface Visitor {
  id: string
  email: string
  image: string
  address: string
  createdAt: string
}

interface Event {
  id: number
  entreprise: string
  nom: string
  lieu: string
  date: string
  heure: string
  createdAt: string
  lien: string
  participation: number
  qrCode: string
  selected: boolean
  dropdown: boolean
  visitors?: Visitor[]
}

// ================== DONNÉES ==================
const activeTab = ref<'list' | 'participants'>('list')
const selectedEvent = ref<Event | null>(null)
const perPage = ref(5)
const currentPage = ref(1)

const events = ref<Event[]>([
  {
    id: 1,
    entreprise: 'Mozar Group',
    nom: 'Forum Digital',
    lieu: 'Abidjan',
    date: '2025-11-01',
    heure: '09:00',
    createdAt: '2025-09-30',
    lien: '#',
    participation: 124,
    qrCode: 'https://api.qrserver.com/v1/create-qr-code/?size=80x80&data=Forum+Digital',
    selected: false,
    dropdown: false
  },
  {
    id: 2,
    entreprise: 'Masek Holding',
    nom: 'Conférence Élite',
    lieu: 'Plateau',
    date: '2025-12-05',
    heure: '14:00',
    createdAt: '2025-10-05',
    lien: '#',
    participation: 58,
    qrCode: 'https://api.qrserver.com/v1/create-qr-code/?size=80x80&data=Conference+Elite',
    selected: false,
    dropdown: false
  }
])

// Dictionnaire des participants par nom d'événement (index signature)
const visitorsMap: Record<string, Visitor[]> = {
  'Forum Digital': [
    { id: 'V001', email: 'john@example.com', image: 'https://randomuser.me/api/portraits/men/1.jpg', address: 'Abidjan, Cocody', createdAt: '2025-10-10' },
    { id: 'V002', email: 'marie@example.com', image: 'https://randomuser.me/api/portraits/women/2.jpg', address: 'Yopougon, Abobo', createdAt: '2025-10-12' },
    { id: 'V003', email: 'patrick@example.com', image: 'https://randomuser.me/api/portraits/men/3.jpg', address: 'Marcory, Zone 4', createdAt: '2025-10-14' }
  ],
  'Conférence Élite': [
    { id: 'V006', email: 'luc@example.com', image: 'https://randomuser.me/api/portraits/men/6.jpg', address: 'Plateau, Abidjan', createdAt: '2025-10-20' },
    { id: 'V007', email: 'sophie@example.com', image: 'https://randomuser.me/api/portraits/women/7.jpg', address: 'Cocody, Riviera', createdAt: '2025-10-21' }
  ]
}

// ================== MÉTHODES ==================
function voirParticipants(event: Event) {
  selectedEvent.value = {
    ...event,
    visitors: visitorsMap[event.nom] ?? []
  }
  activeTab.value = 'participants'
}

function ajouterEvent(event: Event) {
  alert(`Nouvel événement similaire à "${event.nom}"`)
}

function modifierEvent(event: Event) {
  alert(`Modification de l'événement "${event.nom}"`)
}

function supprimerEvent(eventId: number) {
  if (confirm('Supprimer définitivement cet événement ?')) {
    events.value = events.value.filter(e => e.id !== eventId)
  }
}

// Pagination
const totalPages = computed(() => Math.ceil(events.value.length / perPage.value))
const paginatedEvents = computed(() => {
  const start = (currentPage.value - 1) * perPage.value
  return events.value.slice(start, start + perPage.value)
})
</script>

<style scoped>
/* Ajustements Tailwind personnalisés si besoin */
</style>