<script setup>
import { computed } from 'vue';

const props = defineProps({
    alerta: Object,
    required: true,
});
const isError = computed(() => props.alerta.tipo === 'error');
</script>

<template>
    <div :class="[isError ? 'bg-red-500': 'bg-green-500']" class="text-white text-center p-3 font-bold mb-3 rounded-md">
        {{ alerta.mensaje }}
    </div>
</template>
