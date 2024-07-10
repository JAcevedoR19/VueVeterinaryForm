<script setup>
    import { reactive, computed } from 'vue';
    import Alerta from './Alerta.vue';

    const alerta = reactive({
        tipo: '',
        msg: ''
    });

    const emit = defineEmits(['update:nombre', 'update:propietario', 'update:email', 'update:alta', 'update:sintomas', 'enviar-paciente', 'guardar-paciente']);

    const props = defineProps({
        id: {
            type: [String, null],
            required: true
        },
        nombre: {
            type: String,
            required: true
        },
        propietario: {
            type: String,
            required: true
        },
        email: {
            type: String,
            required: true
        },
        alta: {
            type: String,
            required: true
        },
        sintomas: {
            type: String,
            required: true
        }
    });

    const validar = () => {
        if (Object.values(props).includes('')) {
            alerta.msg = 'Todos los campos son obligatorios';
            alerta.tipo = 'error';
            return; 
        }

        emit('guardar-paciente');

        alerta.msg = 'Guardado Correctamente';
        alerta.tipo = 'exito';

        setTimeout(() => {
            alerta.msg = '';
            alerta.tipo = '';
        }, 5000);
    }

    const editando = computed(() => { return props.id });
</script>

<template>
    <div class="md:w-1/2">
        <h2 class="text-3xl font-black text-center">Seguimiento de Pacientes</h2>

        <p class="text-lg mt-5 text-center mb-10">
            Añade Pacientes y <span class="text-indigo-600 font-bold">Adminístralos</span>
        </p>
        
        <Alerta v-if="alerta.msg" :alerta="alerta"/>

        <form class="bg-white shadow-xl rounded-lg py-10 px-5 mb-10" @submit.prevent="validar">
            <div class="mb-5">
                <label for="mascota" class="block text-gray-700 uppercase font-bold">Nombre de la Mascota</label>
                <input id="mascota" type="text" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" placeholder="Nombre de la Mascota" :value="nombre" @input="$emit('update:nombre', $event.target.value)" />
            </div>

            <div class="mb-5">
                <label for="nombreProp" class="block text-gray-700 uppercase font-bold">Nombre del Propietario</label>
                <input id="nombreProp" type="text" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" placeholder="Nombre del Propietario" :value="propietario" @input="$emit('update:propietario', $event.target.value)"/>
            </div>

            <div class="mb-5">
                <label for="emailProp" class="block text-gray-700 uppercase font-bold">Email del Propietario</label>
                <input id="emailProp" type="email" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" placeholder="Email del Propietario" :value="email" @input="$emit('update:email', $event.target.value)"/>
            </div>

            <div class="mb-5">
                <label for="alta" class="block text-gray-700 uppercase font-bold">Alta</label>
                <input id="alta" type="date" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" :value="alta" @input="$emit('update:alta', $event.target.value)"/>
            </div>

            <div class="mb-5">
                <label for="sintomasPaciente" class="block text-gray-700 uppercase font-bold">Motivo de consulta</label>
                <textarea id="sintomasPaciente" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40" placeholder="Describir los sintomas" :value="sintomas" @input="$emit('update:sintomas', $event.target.value)"/>
            </div>

            <input type="submit" class="bg-indigo-600 w-full p-3 text-white uppercase font-bold rounded-md hover:bg-indigo-700 cursor-pointer transition-colors " :value="[editando ? 'Guardar Cambios' : 'Agregar Paciente']"/>
        </form>
    </div>
</template>