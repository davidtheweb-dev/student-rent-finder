<script setup>
import { ref, computed, onBeforeMount } from 'vue';
import { useRequestsStore } from '../../stores/requests/RequestsStore';

import RequestsItem from '../../components/requests/RequestsItem.vue';

const requestsStore = useRequestsStore();

const isLoading = ref(false);
const error = ref(null);

onBeforeMount(() => {
  loadRequests();
});

const receivedRequests = computed(() => {
  return requestsStore.getRequests;
});

const userHasRequests = computed(() => {
  return requestsStore.getUserHasRequests;
});

async function loadRequests() {
  isLoading.value = true;

  try {
    await requestsStore.fetchRequests();
  } catch (err) {
    error.value = err.message || 'Error inesperado al cargar los mensajes';
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
    <section>
      <base-card>
        <header>
          <h2>Mensajes recibidos</h2>
        </header>
        <base-spinner v-if="isLoading"></base-spinner>
        <ul v-else-if="userHasRequests && !isLoading">
          <requests-item
            v-for="req in receivedRequests"
            :key="req.id"
            :email="req.email"
            :message="req.message"
          ></requests-item>
        </ul>
        <h3 v-else>No has recibido ningún mensaje aún</h3>
      </base-card>
    </section>
  </div>
</template>

<style scoped>
header {
  text-align: center;
}

ul {
  list-style: none;
  margin: 2rem auto;
  padding: 0;
  max-width: 30rem;
}

h3 {
  text-align: center;
}
</style>
