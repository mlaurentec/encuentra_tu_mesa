<template>
  <div class="min-h-screen bg-gradient-to-br from-pink-100 to-purple-200 flex items-center justify-center p-4">
    <div class="bg-white rounded-lg shadow-xl p-6 w-full max-w-md">
      <h1 class="text-3xl font-bold text-center text-gray-800 mb-6">Encuentra tu Mesa</h1>

      <div class="relative mb-4">
        <input
          type="text"
          placeholder="Busca tu nombre"
          class="w-full px-4 py-2 rounded-lg border-2 border-pink-300 focus:outline-none focus:border-pink-500 pr-10"
          v-model="searchTerm"
        />
        <!-- Botón "X" para limpiar el campo de búsqueda -->
        <button
          v-if="searchTerm"
          @click="clearSearch"
          class="absolute inset-y-0 right-0 px-3 flex items-center text-gray-500 hover:text-gray-700"
        >
          <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
          </svg>
        </button>

        <ul v-if="suggestions.length" class="absolute z-10 w-full bg-white border border-gray-300 rounded-b-lg shadow-lg max-h-60 overflow-auto">
          <li 
            v-for="(suggestion, index) in suggestions" 
            :key="index" 
            class="px-4 py-2 hover:bg-pink-100 cursor-pointer"
            @click="handleSuggestionClick(suggestion)"
          >
            {{ suggestion }}
          </li>
        </ul>
      </div>

      <div v-if="foundGuest" class="text-center mt-6">
        <p class="text-xl font-semibold text-gray-800 mb-2">{{ foundGuest.name }}</p>
        <img :src="tableImage" alt="Mesa" class="mx-auto mb-4" style="width: 100px; height: 100px;" />
        <p class="text-2xl font-bold text-pink-600">Mesa {{ foundGuest.table }}</p>
      </div>

      <p v-if="searchTerm && !foundGuest && !suggestions.length" class="text-center text-gray-600 mt-4">
        No se encontró el invitado. Intenta de nuevo.
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue';
import tableImage from './assets/mesa.jpg'; // Asegúrate de que la imagen esté aquí

const guestList = [
  { name: "Juan Pérez", table: 1 },
  { name: "María García", table: 2 },
  { name: "Carlos Rodríguez", table: 3 },
  { name: "Ana Martínez", table: 1 },
  { name: "Gloria andrade Ames", table: 2 },
  { name: "Miguel Laurente", table: 2 },
  { name: "Agapito Andrade", table: 2 },
  { name: "Garfield Parras", table: 6 },
  { name: "kitty cochiniti", table: 7 },
  { name: "Mikotas dora", table: 10 },
];

const searchTerm = ref('');
const foundGuest = ref(null);
const suggestions = ref([]);

watch(searchTerm, (newTerm) => {
  if (newTerm.length > 0) {
    const filteredSuggestions = guestList
      .filter(guest => guest.name.toLowerCase().includes(newTerm.toLowerCase()))
      .map(guest => guest.name);
    suggestions.value = filteredSuggestions;
  } else {
    suggestions.value = [];
  }
});

const handleSearch = () => {
  const guest = guestList.find(g => g.name.toLowerCase() === searchTerm.value.toLowerCase());
  foundGuest.value = guest;
  suggestions.value = [];
};

const handleSuggestionClick = (name) => {
  searchTerm.value = name;
  setTimeout(() => {
    handleSearch();
  }, 0);
};

const clearSearch = () => {
  searchTerm.value = '';
  foundGuest.value = null;
  suggestions.value = [];
};
</script>

<style scoped>
/* Aquí puedes agregar estilos específicos si es necesario */
</style>
