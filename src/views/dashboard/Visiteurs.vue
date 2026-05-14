<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'

// Types
interface Employee {
  id: string
  name: string
  email: string
  image: string
  clockIn: string
  clockOut: string
  totalHours: string
  breakTime: string
  overtime: string
  status: string
  dept: string
}

// UI state
const showList = ref(true)
const searchQuery = ref('')
const showFilterPanel = ref(false)
const perPageDropdown = ref(false)
const perPage = ref(15)
const currentPage = ref(1)
const filterStatus = ref('')
const filterDept = ref('')
const activeMenuId = ref<string | null>(null)

// ==================== GESTION DYNAMIQUE DES DATES ====================
// Date courante (initialisée à aujourd'hui)
const currentDateObj = ref(new Date())

// Année sélectionnée (liée à la date courante)
const year = computed({
  get: () => currentDateObj.value.getFullYear(),
  set: (newYear: number) => {
    const newDate = new Date(currentDateObj.value)
    newDate.setFullYear(newYear)
    currentDateObj.value = newDate
  }
})

// Formatage de la date pour l'affichage : "Monday 05 October"
const currentDate = computed(() => {
  const date = currentDateObj.value
  const options: Intl.DateTimeFormatOptions = { weekday: 'long', day: 'numeric', month: 'long' }
  return date.toLocaleDateString('fr-FR', options).replace(/^./, str => str.toUpperCase()) // Première lettre en majuscule
})

// Navigation jour par jour
function prevDay() {
  const newDate = new Date(currentDateObj.value)
  newDate.setDate(newDate.getDate() - 1)
  currentDateObj.value = newDate
}

function nextDay() {
  const newDate = new Date(currentDateObj.value)
  newDate.setDate(newDate.getDate() + 1)
  currentDateObj.value = newDate
}

// Sélecteur d'année (dropdown)
const yearDropdown = ref(false)
function toggleYearDropdown() { yearDropdown.value = !yearDropdown.value }
function selectYear(y: number) {
  year.value = y
  yearDropdown.value = false
}

// Le reste des fonctions (toggleFilter, resetFilters, pagination, etc.) reste identique
function toggleFilter() { showFilterPanel.value = !showFilterPanel.value }
function resetFilters() { filterStatus.value = ''; filterDept.value = '' }
function togglePerPageDropdown() { perPageDropdown.value = !perPageDropdown.value }
function setPerPage(n: number) { perPage.value = n; perPageDropdown.value = false; currentPage.value = 1 }
function goToPage(n: number) {
  if (n < 1) n = 1
  if (n > totalPages.value) n = totalPages.value
  currentPage.value = n
}

// Gestion du menu déroulant des actions
function toggleActions(empId: string) {
  activeMenuId.value = activeMenuId.value === empId ? null : empId
}

// Fermer le menu si on clique ailleurs
function handleClickOutside(event: MouseEvent) {
  const target = event.target as HTMLElement
  if (!target.closest('.relative')) {
    activeMenuId.value = null
  }
}

onMounted(() => {
  document.addEventListener('click', handleClickOutside)
})

onUnmounted(() => {
  document.removeEventListener('click', handleClickOutside)
})

// Actions CRUD simulées
function editEmployee(emp: Employee) {
  alert(`Modifier ${emp.name}`)
  activeMenuId.value = null
}
function deleteEmployee(emp: Employee) {
  alert(`Supprimer ${emp.name}`)
  activeMenuId.value = null
}
function addEmployee(emp: Employee) {
  alert(`Ajouter ${emp.name} comme nouvel employé`)
  activeMenuId.value = null
}

// Données mock (inchangées)
const employees = ref<Employee[]>([
  { id: 'E01', name: 'Jerome Bell', email: 'jerome@alignui.com', image: 'https://randomuser.me/api/portraits/men/1.jpg', clockIn: '10:02 AM', clockOut: '09:10 PM', totalHours: '8h 58m', breakTime: '10:00–10:15', overtime: '2h 10', status: 'On time', dept: 'Engineering' },
  { id: 'E02', name: 'Liam Carter', email: 'liam@alignui.com', image: 'https://randomuser.me/api/portraits/men/2.jpg', clockIn: '12:02 PM', clockOut: '09:00 PM', totalHours: '8h 58m', breakTime: '11:00–11:10', overtime: '-', status: 'Late', dept: 'Sales' },
  { id: 'E03', name: 'Maya Ross', email: 'maya@alignui.com', image: 'https://randomuser.me/api/portraits/women/3.jpg', clockIn: '10:02 AM', clockOut: '09:10 PM', totalHours: '8h 58m', breakTime: '11:00–11:10', overtime: '2h 10', status: 'Late', dept: 'Design' },
  { id: 'E04', name: 'Ethan Cole', email: 'ethan@alignui.com', image: 'https://randomuser.me/api/portraits/men/4.jpg', clockIn: '10:02 AM', clockOut: '09:10 PM', totalHours: '8h 58m', breakTime: '12:00–12:20', overtime: '2h 10', status: 'On time', dept: 'Engineering' },
  { id: 'E05', name: 'Ava Brooks', email: 'ava@alignui.com', image: 'https://randomuser.me/api/portraits/women/5.jpg', clockIn: '10:02 AM', clockOut: '07:00 PM', totalHours: '8h 58m', breakTime: '10:00–10:15', overtime: '-', status: 'On time', dept: 'Sales' },
  { id: 'E06', name: 'Noah Reed', email: 'noah@alignui.com', image: 'https://randomuser.me/api/portraits/men/6.jpg', clockIn: '10:02 AM', clockOut: '09:10 PM', totalHours: '8h 58m', breakTime: '11:00–11:10', overtime: '2h 10', status: 'On time', dept: 'Design' },
  { id: 'E07', name: 'Chloe Dean', email: 'chloe@alignui.com', image: 'https://randomuser.me/api/portraits/women/7.jpg', clockIn: '10:02 AM', clockOut: '07:00 PM', totalHours: '8h 58m', breakTime: '12:00–12:20', overtime: '-', status: 'On time', dept: 'Engineering' },
  { id: 'E08', name: 'Owen Hayes', email: 'owen@alignui.com', image: 'https://randomuser.me/api/portraits/men/8.jpg', clockIn: '10:02 AM', clockOut: '09:10 PM', totalHours: '8h 58m', breakTime: '10:00–10:15', overtime: '2h 10', status: 'On time', dept: 'Sales' },
  { id: 'E09', name: 'Zara Lane', email: 'zara@alignui.com', image: 'https://randomuser.me/api/portraits/women/9.jpg', clockIn: '09:30 AM', clockOut: '06:30 PM', totalHours: '9h 0m', breakTime: '11:30–11:50', overtime: '-', status: 'Late', dept: 'Design' },
  { id: 'E10', name: 'Lucas Ford', email: 'lucas@alignui.com', image: 'https://randomuser.me/api/portraits/men/10.jpg', clockIn: '09:30 AM', clockOut: '06:30 PM', totalHours: '9h 0m', breakTime: '11:30–11:50', overtime: '-', status: 'Late', dept: 'Design' },
])

// Filtres + pagination
const filteredEmployees = computed(() => {
  return employees.value.filter(emp =>
    emp.name.toLowerCase().includes(searchQuery.value.toLowerCase()) &&
    (filterStatus.value ? emp.status === filterStatus.value : true) &&
    (filterDept.value ? emp.dept === filterDept.value : true)
  )
})

const totalPages = computed(() => Math.ceil(filteredEmployees.value.length / perPage.value))

const paginatedEmployees = computed(() => {
  const start = (currentPage.value - 1) * perPage.value
  return filteredEmployees.value.slice(start, start + perPage.value)
})

const pageNumbers = computed(() => Array.from({ length: totalPages.value }, (_, i) => i + 1))
</script>