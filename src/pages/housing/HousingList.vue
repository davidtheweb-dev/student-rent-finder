<script setup>
import { ref, reactive, computed, onBeforeMount } from 'vue';
import { useAuthStore } from '../../stores/auth/AuthStore';
import { useHousingStore } from '../../stores/housing/HousingStore';

import HousingItem from '../../components/housing/HousingItem.vue';
import TheFilter from '../../components/ui/layout/TheFilter.vue';

const authStore = useAuthStore();
const housingStore = useHousingStore();

const isLoading = ref(false);
const error = ref(null);

const activeFilters = reactive({
  lgtb: true,
  bath: true,
  couples: true,
});

onBeforeMount(() => {
  loadHousing();
});

const isLoggedIn = computed(() => {
  return authStore.getIsAuthenticated;
});

const filterHasHousing = computed(() => {
  return !isLoading.value && filteredHousing.value.length > 0;
});

const userHasHousing = computed(() => {
  return housingStore.getUserHasHousing;
});

const filteredHousing = computed(() => {
  const housing = housingStore.getHousing;
  // return housing.filter((housing) => {
  //   if (activeFilters.lgtb && housing.tags.includes('LGTB friendly')) {
  //     return true;
  //   }
  //   if (activeFilters.bath && housing.tags.includes('Baño privado')) {
  //     return true;
  //   }
  //   if (activeFilters.couples && housing.tags.includes('Admite parejas')) {
  //     return true;
  //   }
  //   return false;
  // });
  return housing;
});

function setFilters(updatedFilters) {
  Object.keys(updatedFilters).forEach((key) => {
    activeFilters[key] = updatedFilters[key];
  });
}

async function loadHousing(refresh = false) {
  isLoading.value = true;

  try {
    await housingStore.fetchHousing(refresh);
  } catch (err) {
    error.value = err.message || 'Error inesperado al cargar las viviendas';
  }

  isLoading.value = false;
}

function handleDialogError() {
  error.value = null;
}
</script>

<template>
  <div>
    <base-dialog
      :show="!!error"
      title="Por favor, contacta con soporte indicando el error"
      @close="handleDialogError"
    >
      <p>{{ error }}</p>
    </base-dialog>
    <section class="sticky -top-20">
      <the-filter @change-filter="setFilters"></the-filter>
    </section>
    <section>
      <base-card>
        <div class="flex justify-between">
          <base-button mode="outline" @click="loadHousing(true)">Actualizar</base-button>
          <base-button v-if="!isLoggedIn" link to="/autenticacion?redirect=registro"
            >¡Sube tu piso!</base-button
          >
          <base-button v-if="isLoggedIn && !userHasHousing && !isLoading" link to="/registro-piso"
            >¡Sube tu piso!</base-button
          >
        </div>
        <div v-if="isLoading">
          <base-spinner></base-spinner>
        </div>
        <ul v-else-if="filterHasHousing">
          <housing-item
            v-for="housing in filteredHousing"
            :key="housing.id"
            :housing="housing"
          ></housing-item>
        </ul>
        <h3 v-else>No se han encontrado viviendas</h3>
      </base-card>
    </section>
  </div>
</template>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
