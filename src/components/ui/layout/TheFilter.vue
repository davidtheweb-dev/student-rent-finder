<template>
  <base-card>
    <span class="filter-option">
      <base-button link to="/pisos" :mode="buttonMode.housing">Habitaciones</base-button>
      <base-button link to="/companeros" :mode="buttonMode.partner">Compañeros</base-button>
    </span>
    <span class="filter-option gap">
      <div><i class="fa-solid fa-filter fa-xl"></i><button class="filter-button"></button></div>
      <base-like id="filter"></base-like>
    </span>

    <!-- <span class="filter-option">
      <input id="lgtb" type="checkbox" checked @change="setFilter" />
      <label for="lgtb">LGTB friendly</label>
    </span>
    <span class="filter-option">
      <input id="bath" type="checkbox" checked @change="setFilter" />
      <label for="bath">Baño privado</label>
    </span>
    <span class="filter-option">
      <input id="couples" type="checkbox" checked @change="setFilter" />
      <label for="couples">Admite parejas</label>
    </span> -->
  </base-card>
</template>

<script setup>
// const emit = defineEmits(['change-filter']);

// let filters = {
//   lgtb: true,
//   bath: true,
//   couples: true,
// };

// function setFilter(event) {
//   const inputId = event.target.id;
//   const isActive = event.target.checked;

//   const updatedFilters = {
//     ...filters,
//     [inputId]: isActive, // Overrides the filter that changed (inputId) and its new value (isActive)
//   };

//   filters = updatedFilters;
//   emit('change-filter', updatedFilters);
// }

import { onMounted, reactive } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();

const buttonMode = reactive({
  housing: null,
  partner: null,
});

onMounted(() => {
  checkMode();
});

function checkMode() {
  if (route.path === '/pisos') {
    buttonMode.housing = '';
    buttonMode.partner = 'outline';
  } else {
    buttonMode.partner = '';
    buttonMode.housing = 'outline';
  }
}
</script>

<style scoped>
.filter-option {
  padding: 0.7rem;
  display: flex;
  justify-content: center;
  align-items: center;
}

.gap {
  gap: 1rem;
}

.filter-button {
  font-weight: bold;
  color: var(--color-white-100);
  background-color: transparent;
  border: none;
}
</style>
