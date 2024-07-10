<script setup>
    import { computed } from 'vue';
    const props = defineProps({
        alerta: Object,
        required: true
    });

    const isError = computed(() => {
        return props.alerta.tipo === 'error';
    });
</script>

<template>
    <div class="text-white text-center p-3 uppercase font-bold mb-3 rounded-md" 
    :class="[isError ? 'bg-red-500' : 'bg-green-500']">
        {{ alerta.msg }}
    </div>
</template>
