<script setup>
defineProps({
    patient: {
        type: Object,
        required: true
    }
});

defineEmits(['update-patient', 'delete-patient']);
</script>

<template>
     <div class="card bg-base-200 mt-4 shadow-sm mx-5 flex flex-col sm:flex-row">
        <figure class="sm:w-1/3">
            <img
                src="../assets/img/momo-studio.jpg"
                class="h-full w-full object-cover"
                alt="pet" />
        </figure>
        <div class="card-body sm:w-2/3 overflow-hidden text-ellipsis">
            <h2 class="card-title text-xl">{{ patient.name }}</h2>
            <ul class="list-none p-0">
                <li class="p-2 border-b border-gray-200"><strong>Propietario:</strong> {{ patient.owner }}</li>
                <li class="p-2 border-b border-gray-200"><strong>Correo:</strong> {{ patient.email }}</li>
                <li class="p-2 border-b border-gray-200"><strong>Fecha:</strong> {{ patient.date }}</li>
                <li class="p-2 border-b border-gray-200"><strong>Síntomas:</strong> {{ patient.symptom }}</li>
            </ul>
            <div class="grid md:grid-cols-2  gap-5">
                <button class="btn btn-primary" v-on:click="$emit('update-patient', patient.id)">Actualizar</button>
                <button class="btn btn-error"  v-on:click="$emit('delete-patient', patient.id)">Eliminar</button>
            </div>
        </div>
    </div>
</template>