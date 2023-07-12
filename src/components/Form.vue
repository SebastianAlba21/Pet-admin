<script setup>
import { computed, reactive } from 'vue';
import Alerta from './Alerta.vue';

const alerta = reactive({
    mensaje: '',
    tipo: '',
});

const emit = defineEmits(['update:nombre', 'update:propietario', 'update:email', 'update:alta', 'update:sintomas', 'guardar-paciente'])

const props = defineProps({
    id: {
        type: [String, null],
        required: true,
    },
    nombre: {
        type: String,
        required: true,
    },
    propietario: {
        type: String,
        required: true,
    },
    email: {
        type: String,
        required: true,
    },
    alta: {
        type: String,
        required: true,
    },
    sintomas: {
        type: String,
        required: true,
    },
});

const validar = () => {
    if (Object.values(props).includes('')) {
        alerta.mensaje = 'Todos los campos son obligatorios';
        alerta.tipo = 'error';
        return
    }
    emit('guardar-paciente');
    alerta.mensaje = 'Paciente agregado correctamente';
    alerta.tipo = 'exito';
    setTimeout(() => {
        Object.assign(alerta, {
            mensaje: '',
            tipo: '',
        });
    }, 2000);
}

const editando = computed(() => {
    return props.id
})
</script>

<template>
    <div class="md:w-1/2">
        <h2 class="font-black text-3xl text-center">Seguimiento de pacientes</h2>
        <p class="text-lg mt-5 text-center mb-10">
            Añade pacientes y
            <span class="text-indigo-600 font-bold">Administralos</span>
        </p>
        <Alerta v-if="alerta.mensaje" :alerta="alerta" />
        <form class="bg-white shadow-md rounded-lg py-10 px-5 mb-10" @submit.prevent="validar">
            <div class="mb-5">
                <label class="block text-gray-700 uppercase font-bold" for="mascota">
                    Nombre Mascota
                </label>
                <input type="text" placeholder="Nombre de la mascota"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" id="mascota" :value="nombre"
                    @input="$emit('update:nombre', $event.target.value)">
            </div>
            <div class="mb-5">
                <label class="block text-gray-700 uppercase font-bold" for="propietario">
                    Nombre Propietario
                </label>
                <input type="text" placeholder="Nombre del propietario"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" id="propietario" :value="propietario"
                    @input="$emit('update:propietario', $event.target.value)">
            </div>
            <div class="mb-5">
                <label class="block text-gray-700 uppercase font-bold" for="email">
                    Email
                </label>
                <input type="text" placeholder="Email del propietario"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" id="email" :value="email"
                    @input="$emit('update:email', $event.target.value)">
            </div>
            <div class="mb-5">
                <label class="block text-gray-700 uppercase font-bold" for="alta">
                    Alta
                </label>
                <input type="date" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" id="alta"
                    @input="$emit('update:alta', $event.target.value)" :value="alta">
            </div>
            <div class="mb-5">
                <label class="block text-gray-700 uppercase font-bold" for="sintomas">
                    Sintomas
                </label>
                <textarea id="sintomas" placeholder="Describe los sintomas"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40"
                    @input="$emit('update:sintomas', $event.target.value)" :value="sintomas" />
            </div>
            <input type="submit" :value="[editando ? 'Guardar cambios' : 'Agregar paciente']"
                class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo-800 cursor-pointer transition-colors rounded-lg">
        </form>
    </div>
</template>



