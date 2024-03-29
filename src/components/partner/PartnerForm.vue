<script setup>
import { reactive, computed, onBeforeMount } from 'vue';

const props = defineProps({
  partnerInfo: {
    type: Object,
    default: () => ({
      registration: true,
    }),
  },
});

const emit = defineEmits(['save-data']);

// eslint-disable-next-line vue/no-setup-props-destructure
const partnerInfoLocal = { ...props.partnerInfo[0] };

onBeforeMount(() => {
  if (!props.partnerInfo.registration) {
    title.value = partnerInfoLocal.title;
    description.value = partnerInfoLocal.description;
    genre.value = partnerInfoLocal.genre;
    rate.value = partnerInfoLocal.rate;
    area.value = partnerInfoLocal.area;
    startMonth.value = partnerInfoLocal.startMonth;
    monthsDuration.value = partnerInfoLocal.monthsDuration;
    isShared.value = partnerInfoLocal.isShared;
    numPartners.value = partnerInfoLocal.numPartners;
    freePlaces.value = partnerInfoLocal.freePlaces;
    typeRoom.value = partnerInfoLocal.typeRoom;
    typeBed.value = partnerInfoLocal.typeBed;
    isSmoke.value = partnerInfoLocal.isSmoke;
    isPet.value = partnerInfoLocal.isPet;
    instagram.value = partnerInfoLocal.instagram;
    whatsapp.value = partnerInfoLocal.whatsapp;
    tags.value = partnerInfoLocal.tags;
  }
});

const monthsOptions = computed(() => {
  let months = [];
  for (let i = 0; i < 6; i++) {
    let today = new Date();
    today.setMonth(today.getMonth() + i);
    let thisMonth = today.toLocaleString('es-ES', {
      month: 'long',
    });

    const upperThisMonth = thisMonth.charAt(0).toUpperCase() + thisMonth.slice(1);

    months.push(upperThisMonth);
  }
  return months;
});

const title = reactive({
  value: '',
  isValid: true,
});
const description = reactive({
  value: '',
  isValid: true,
});
const genre = reactive({
  value: '',
  isValid: true,
});
const rate = reactive({
  value: null,
  isValid: true,
});
const area = reactive({
  value: '',
  isValid: true,
});
const startMonth = reactive({
  value: '',
  isValid: true,
});
const monthsDuration = reactive({
  value: null,
  isValid: true,
});
const isShared = reactive({
  value: null,
  isValid: true,
});
const numPartners = reactive({
  value: null,
  isValid: true,
});
const freePlaces = reactive({
  value: null,
  isValid: true,
});
const typeRoom = reactive({
  value: null,
  isValid: true,
});
const typeBed = reactive({
  value: null,
  isValid: true,
});
const isSmoke = reactive({
  value: null,
  isValid: true,
});
const isPet = reactive({
  value: null,
  isValid: true,
});
const instagram = reactive({
  value: '',
  isValid: true,
});
const whatsapp = reactive({
  value: '',
  isValid: true,
});
const tags = reactive({
  value: [],
  isValid: true,
});
let formIsValid = true;

function clearValidity(input) {
  input.isValid = true;
}

function validateForm() {
  formIsValid = true;
  if (title.value === '') {
    title.isValid = false;
    formIsValid = false;
  }

  if (description.value === '') {
    description.isValid = false;
    formIsValid = false;
  }

  if (genre.value === '') {
    genre.isValid = false;
    formIsValid = false;
  }

  if (!rate.value || rate.value <= 0) {
    rate.isValid = false;
    formIsValid = false;
  }

  if (area.value === '') {
    area.isValid = false;
    formIsValid = false;
  }

  if (startMonth.value === '') {
    startMonth.isValid = false;
    formIsValid = false;
  }

  if (!monthsDuration.value || monthsDuration.value <= 0) {
    monthsDuration.isValid = false;
    formIsValid = false;
  }

  if (isShared.value === null) {
    isShared.isValid = false;
    formIsValid = false;
  }

  if (numPartners.value === null && isShared.value) {
    numPartners.isValid = false;
    formIsValid = false;
  }

  if (freePlaces.value === null && isShared.value) {
    freePlaces.isValid = false;
    formIsValid = false;
  }

  if (typeRoom.value === null && isShared.value) {
    typeRoom.isValid = false;
    formIsValid = false;
  }

  if (typeBed.value === null) {
    typeBed.isValid = false;
    formIsValid = false;
  }

  if (isSmoke.value === null) {
    isSmoke.isValid = false;
    formIsValid = false;
  }

  if (isPet.value === null) {
    isPet.isValid = false;
    formIsValid = false;
  }

  if (tags.value.length === 0) {
    tags.isValid = false;
    formIsValid = false;
  }
}

function submitForm() {
  validateForm();

  if (!formIsValid) {
    return;
  }

  const formData = {
    title: title.value,
    description: description.value,
    genre: genre.value,
    rate: rate.value,
    area: area.value,
    startMonth: startMonth.value,
    monthsDuration: monthsDuration.value,
    isShared: isShared.value,
    numPartners: numPartners.value,
    freePlaces: freePlaces.value,
    typeRoom: typeRoom.value,
    typeBed: typeBed.value,
    isSmoke: isSmoke.value,
    isPet: isPet.value,
    instagram: instagram.value,
    whatsapp: whatsapp.value,
    tags: tags.value,
  };

  emit('save-data', formData);
}
</script>

<template>
  <form @submit.prevent="submitForm">
    <div :class="{ invalid: !title.isValid }">
      <label for="title">Título</label>
      <input id="title" v-model.trim="title.value" type="text" @blur="clearValidity(title)" />
      <p v-if="!title.isValid" class="font-bold text-red-600">El título no puede estar vacío</p>
    </div>
    <div :class="{ invalid: !description.isValid }">
      <label for="description">Descripción</label>
      <textarea
        id="description"
        v-model.trim="description.value"
        rows="5"
        @blur="clearValidity(description)"
      ></textarea>
      <p v-if="!description.isValid" class="font-bold text-red-600">
        La descripción no puede estar vacía
      </p>
    </div>
    <div :class="{ invalid: !genre.isValid }">
      <label for="genre">Soy</label>
      <select id="genre" v-model.trim="genre.value" name="genre" @blur="clearValidity(genre)">
        <option value=""></option>
        <option value="boy">Chico</option>
        <option value="girl">Chica</option>
        <option value="other">Otro</option>
      </select>
      <p v-if="!genre.isValid" class="font-bold text-red-600">Selecciona una opción</p>
    </div>
    <div :class="{ invalid: !rate.isValid }">
      <label for="rate">Presupuesto mensual máximo (€)</label>
      <input id="rate" v-model.number="rate.value" type="number" @blur="clearValidity(rate)" />
      <p v-if="!rate.isValid" class="font-bold text-red-600">El precio debe ser mayor de 0</p>
    </div>
    <div :class="{ invalid: !area.isValid }">
      <label for="area">Zona</label>
      <input id="area" v-model.trim="area.value" type="text" @blur="clearValidity(area)" />
      <p v-if="!area.isValid" class="font-bold text-red-600">La zona no puede estar vacía</p>
    </div>
    <div :class="{ invalid: !startMonth.isValid }">
      <label for="startMonth">Busco piso para</label>
      <select
        id="startMonth"
        v-model.trim="startMonth.value"
        name="startMonth"
        @blur="clearValidity(startMonth)"
      >
        <option value=""></option>
        <option v-for="month in monthsOptions" :key="month" :value="month">{{ month }}</option>
      </select>
      <p v-if="!startMonth.isValid" class="font-bold text-red-600">Selecciona una opción</p>
    </div>
    <div :class="{ invalid: !monthsDuration.isValid }">
      <label for="monthsDuration">Quiero quedarme (meses)</label>
      <input
        id="monthsDuration"
        v-model.number="monthsDuration.value"
        type="number"
        @blur="clearValidity(monthsDuration)"
      />
      <p v-if="!monthsDuration.isValid" class="font-bold text-red-600">
        El número de meses debe ser mayor de 0
      </p>
    </div>
    <div :class="{ invalid: !isShared.isValid }">
      <label for="isShared">Busco piso compartido con</label>
      <select
        id="isShared"
        v-model.trim="isShared.value"
        name="isShared"
        @blur="clearValidity(isShared)"
      >
        <option :value="null"></option>
        <option :value="true">Otros estudiántes y jóvenes</option>
        <option :value="false">Estudio o apartamento pequeño (yo solo)</option>
      </select>
      <p v-if="!isShared.isValid" class="font-bold text-red-600">Selecciona una opción</p>
    </div>
    <div v-if="isShared.value">
      <div :class="{ invalid: !numPartners.isValid }">
        <label for="numPartners">Busco compartir piso con</label>
        <select
          id="numPartners"
          v-model.trim="numPartners.value"
          name="numPartners"
          @blur="clearValidity(numPartners)"
        >
          <option :value="null"></option>
          <option :value="1">1 persona</option>
          <option :value="2">2 personas</option>
          <option :value="3">3 o más personas</option>
          <option :value="0">Indiferente</option>
        </select>
        <p v-if="!numPartners.isValid" class="font-bold text-red-600">Selecciona una opción</p>
      </div>
      <div :class="{ invalid: !freePlaces.isValid }">
        <label for="freePlaces">Nº total de plazas libres en un mismo piso</label>
        <select
          id="freePlaces"
          v-model.trim="freePlaces.value"
          name="freePlaces"
          @blur="clearValidity(freePlaces)"
        >
          <option :value="null"></option>
          <option :value="1">1</option>
          <option :value="2">2</option>
          <option :value="3">3</option>
          <option :value="4">4</option>
          <option :value="5">5</option>
          <option :value="6">6</option>
          <option :value="7">Más de 6</option>
          <option :value="0">Indiferente</option>
        </select>
        <p v-if="!typeRoom.isValid" class="font-bold text-red-600">Selecciona una opción</p>
      </div>
      <div :class="{ invalid: !typeRoom.isValid }">
        <label for="typeRoom">Busco una habitación</label>
        <select
          id="typeRoom"
          v-model.trim="typeRoom.value"
          name="typeRoom"
          @blur="clearValidity(typeRoom)"
        >
          <option :value="null"></option>
          <option :value="1">Individual</option>
          <option :value="2">Doble</option>
          <option :value="0">Indiferente</option>
        </select>
        <p v-if="!typeRoom.isValid" class="font-bold text-red-600">Selecciona una opción</p>
      </div>
    </div>
    <div :class="{ invalid: !typeBed.isValid }">
      <label for="typeBed">Quiero una cama</label>
      <select
        id="typeBed"
        v-model.trim="typeBed.value"
        name="typeBed"
        @blur="clearValidity(typeBed)"
      >
        <option :value="null"></option>
        <option :value="1">Individual</option>
        <option :value="2">Doble</option>
        <option :value="0">Indiferente</option>
      </select>
      <p v-if="!typeBed.isValid" class="font-bold text-red-600">Selecciona una opción</p>
    </div>
    <div :class="{ invalid: !isSmoke.isValid }">
      <label for="isSmoke">¿Fumo?</label>
      <select
        id="isSmoke"
        v-model.trim="isSmoke.value"
        name="isSmoke"
        @blur="clearValidity(isSmoke)"
      >
        <option :value="null"></option>
        <option :value="true">Soy fumador</option>
        <option :value="false">No fumador</option>
      </select>
      <p v-if="!isSmoke.isValid" class="font-bold text-red-600">Selecciona una opción</p>
    </div>
    <div :class="{ invalid: !isPet.isValid }">
      <label for="isPet">¿Tengo mascotas?</label>
      <select id="isPet" v-model.trim="isPet.value" name="isPet" @blur="clearValidity(isPet)">
        <option :value="null"></option>
        <option :value="true">Con mascotas</option>
        <option :value="false">Sin mascotas</option>
      </select>
      <p v-if="!isPet.isValid" class="font-bold text-red-600">Selecciona una opción</p>
    </div>

    <div>
      <label for="instagram">Instagram (opcional)</label>
      <input id="instagram" v-model.trim="instagram.value" type="text" placeholder="tinder.urjc" />
    </div>

    <div>
      <label for="whatsapp">WhatsApp (opcional)</label>
      <input id="whatsapp" v-model.number="whatsapp.value" type="number" placeholder="605347867" />
    </div>

    <div :class="{ invalid: !tags.isValid }">
      <h3>Características que necesito</h3>
      <div>
        <input
          id="elevator"
          v-model="tags.value"
          type="checkbox"
          value="elevator"
          @blur="clearValidity(tags)"
        />
        <label for="elevator">Ascensor</label>
      </div>

      <div>
        <input
          id="couples"
          v-model="tags.value"
          type="checkbox"
          value="couples"
          @blur="clearValidity(tags)"
        />
        <label for="couples">Admite parejas</label>
      </div>

      <div>
        <input
          id="air"
          v-model="tags.value"
          type="checkbox"
          value="air"
          @blur="clearValidity(tags)"
        />
        <label for="air">Climatizador</label>
      </div>

      <div>
        <input
          id="internet"
          v-model="tags.value"
          type="checkbox"
          value="internet"
          @blur="clearValidity(tags)"
        />
        <label for="internet">Internet WiFi</label>
      </div>

      <div>
        <input
          id="cleaner"
          v-model="tags.value"
          type="checkbox"
          value="cleaner"
          @blur="clearValidity(tags)"
        />
        <label for="cleaner">Limpieza incluida</label>
      </div>

      <div>
        <input
          id="bath"
          v-model="tags.value"
          type="checkbox"
          value="bath"
          @blur="clearValidity(tags)"
        />
        <label for="bath">Baño privado</label>
      </div>

      <div>
        <input
          id="lgtb"
          v-model="tags.value"
          type="checkbox"
          value="lgtb"
          @blur="clearValidity(tags)"
        />
        <label for="lgtb">LGTB friendly</label>
      </div>

      <p v-if="!tags.isValid" class="font-bold text-red-600">
        Debes seleccionar al menos una característica
      </p>
    </div>

    <p v-if="!formIsValid">Por favor, corrige los campos y vuelve a intentarlo</p>
    <base-button>Guardar</base-button>
  </form>
</template>

<style scoped>
label {
  font-weight: bold;
  display: block;
  margin-bottom: 0.5rem;
}

input[type='checkbox'] + label {
  font-weight: normal;
  display: inline;
  margin: 0 0 0 0.5rem;
}

input,
textarea {
  display: block;
  width: 100%;
  border: 1px solid var(--color-white-100);
  font: inherit;
}

input:focus,
textarea:focus {
  outline: none;
  border-color: var(--color-primary-600);
}

input[type='checkbox'] {
  display: inline;
  width: auto;
  border: none;
}

input[type='checkbox']:focus {
  outline: var(--color-primary-600) solid 1px;
}

h3 {
  margin: 0.5rem 0;
  font-size: 1rem;
}

.invalid label {
  color: red;
}

.invalid input,
.invalid textarea {
  border: 1px solid red;
}
</style>
