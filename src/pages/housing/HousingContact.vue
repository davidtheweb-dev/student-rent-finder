<script setup>
import { ref } from 'vue';
import { useRequestsStore } from '../../stores/requests/RequestsStore';
import { useRoute, useRouter } from 'vue-router';

const requestsStore = useRequestsStore();
const route = useRoute();
const router = useRouter();

const email = ref('');
const message = ref('');
const formIsValid = ref(true);

function submitForm() {
  formIsValid.value = true;
  if (email.value === '' || !email.value.includes('@') || message.value === '') {
    formIsValid.value = false;
    return;
  }

  requestsStore.addRequest({
    email: email.value,
    message: message.value,
    housingId: route.params.id,
  });

  router.replace('/pisos');
}
</script>

<template>
  <form @submit.prevent="submitForm">
    <div>
      <label for="email">Correo electrónico</label>
      <input id="email" v-model.trim="email" type="email" />
    </div>
    <div class="my-2">
      <label for="message">Mensaje</label>
      <textarea id="message" v-model.trim="message" rows="5"></textarea>
    </div>
    <p v-if="!formIsValid" class="font-bold text-red-600">
      Por favor, introduce un email y mensaje válidos
    </p>
    <div class="text-center">
      <base-button>Envía tu mensaje</base-button>
    </div>
  </form>
</template>

<style scoped>
form {
  margin: 1rem auto 1rem auto;
  border: 1px solid var(--color-surface-600);
  border-radius: 12px;
  padding: 2rem 1.5rem 2rem 1.5rem;
}

label {
  font-weight: bold;
  margin-bottom: 0.5rem;
  display: block;
}

input,
textarea {
  display: block;
  width: 100%;
  font: inherit;
  border: 1px solid var(--color-white-100);
  padding: 0.15rem;
  margin-bottom: 1.5rem;
  border-radius: 3px;
}

input:focus,
textarea:focus {
  border-color: var(--color-primary-600);
  outline: none;
}
</style>
