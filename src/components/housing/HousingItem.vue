<template>
  <li>
    <h3 data-cy="housing-item-title">{{ title }}</h3>
    <h4 data-cy="housing-item-rate">{{ rate }}€/mes</h4>
    <div>
      <base-badge v-for="tag in tags" :key="tag" :type="tag" :title="tag"></base-badge>
    </div>
    <div class="actions">
      <base-button data-cy="housing-item-contact-btn" mode="outline" link :to="housingContactLink"
        >Contacto</base-button
      >
      <base-button data-cy="housing-item-show-btn" link :to="housingDetailLink"
        >Ver más</base-button
      >
    </div>
  </li>
</template>

<script setup>
import { computed } from 'vue';
import { useRoute } from 'vue-router';

const props = defineProps({
  id: {
    type: String,
    default: 'error',
  },
  title: {
    type: String,
    default: 'Error al cargar el nombre del piso',
  },
  rate: {
    type: Number,
    default: null,
  },
  tags: {
    type: Array,
    default: null,
  },
});

const route = useRoute();

const housingContactLink = computed(() => {
  return route.path + '/' + props.id + '/contacto';
});

const housingDetailLink = computed(() => {
  return route.path + '/' + props.id;
});
</script>

<style scoped>
li {
  margin: 1rem 0;
  border: 1px solid var(--color-surface-600);
  border-radius: 12px;
  padding: 1rem;
}

h3 {
  font-size: 1.5rem;
}

h3,
h4 {
  margin: 0.5rem 0;
}

div {
  margin: 0.5rem 0;
}

.actions {
  display: flex;
  justify-content: flex-end;
}
</style>
