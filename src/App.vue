<script setup>
import { ref, reactive, watch, onMounted } from 'vue';
import { uid } from 'uid';
import Header from './components/Header.vue';
import Form from './components/Form.vue';
import Paciente from './components/Paciente.vue';

const pacientes = ref([]);
const paciente = reactive({
  id: null,
  nombre: '',
  propietario: '',
  email: '',
  alta: '',
  sintomas: '',
});

watch(pacientes, () => {
  guardarLocalStorage();
}, { deep: true })

const guardarLocalStorage = () => localStorage.setItem('pacientes', JSON.stringify(pacientes.value));

onMounted(() => {
  const pacientesLocalStorage = JSON.parse(localStorage.getItem('pacientes'));
  if (pacientesLocalStorage) {
    pacientes.value = pacientesLocalStorage;
  }
});

const guardarPaciente = () => {
  if (paciente.id) {
    const { id } = paciente;
    const index = pacientes.value.findIndex(paciente => paciente.id === id);
    pacientes.value[index] = { ...paciente };
  } else {
    pacientes.value.push({ ...paciente, id: uid() })
  }
  Object.assign(paciente, {
    nombre: '',
    propietario: '',
    email: '',
    alta: '',
    sintomas: '',
  });
};

const actualizarPaciente = (id) => {
  const pacienteEditar = pacientes.value.filter(paciente => paciente.id === id)[0];
  Object.assign(paciente, pacienteEditar);
};

const eliminarPaciente = (id) => pacientes.value = pacientes.value.filter(paciente => paciente.id !== id);

</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />
  </div>
  <div class="mt-12 md:flex">
    <Form v-model:nombre="paciente.nombre" v-model:propietario="paciente.propietario" v-model:email="paciente.email"
      v-model:alta="paciente.alta" v-model:sintomas="paciente.sintomas" @guardar-paciente="guardarPaciente"
      :id="paciente.id" />
    <div class="md:w-1/2 md:h-screen overflow-y-scroll">
      <h3 class="font-black text-3xl text-center">Administra los pacientes</h3>
      <div v-if="pacientes.length > 0">
        <p class="text-lg mt-5 text-center mb-10">
          Infomración de
          <span class="text-indigo-600 font-bold">Pacientes</span>
        </p>
        <Paciente v-for="paciente in pacientes" :paciente="paciente" @actualizar-paciente="actualizarPaciente"
          @eliminar-paciente="eliminarPaciente" />
      </div>
      <p v-else class="mt-20 text-2xl text-center font-bold">No hay pacientes</p>
    </div>
  </div>
</template>

