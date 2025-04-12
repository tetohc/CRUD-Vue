<script setup>
import { reactive, computed } from 'vue';
import Alert from './Alert.vue';

const props = defineProps({
    id: {
        type: [String, null],
        required: true
    },
    name: {
        type: String,
        required: true
    },
    owner: {
        type: String,
        required: true
    },
    email: {
        type: String,
        required: true
    },
    date: {
        type: String,
        required: true
    },
    symptom: {
        type: String,
        required: true
    }
});

const emit = defineEmits(['update:name', 'update:owner', 'update:email', 'update:date',
    'update:symptom', 'save-data']);

const alert = reactive({
    type: '',
    message: ''
});

const validateForm = () => {
    if (Object.values(props).includes('')) {
        alert.type = 'error';
        alert.message = 'Todos los campos son requeridos.';
    } else {
        emit('save-data');
    }
    setTimeout(() => {
        Object.assign(alert, {
            type: '',
            message: ''
        });
    }, 3000);
}

const isEditing = computed(() => !!props.id);

const textLabelForm = computed(() => {
    return isEditing.value ? 'Actualización de paciente' : 'Agregar paciente';
});

const textButton = computed(() => {
    return isEditing.value ? 'Guardar cambios' : 'Registrar paciente';
});

</script>
<template>
    <div class="w-full md:w-1/2 mt-1 p-5">
        <form @submit.prevent="validateForm">
            <Alert 
                v-if="alert.message" 
                :alert="alert" />

            <fieldset class="fieldset w-full bg-base-200 border border-base-300 p-4 rounded-box">
                <legend class="fieldset-legend text-xl">{{ textLabelForm }}</legend>

                <label class="fieldset-label text-lg" for="pet">Nombre de la mascota</label>
                <input type="text" id="pet" class="input w-full" placeholder="Nombre de mascota"
                    :value="name" @input="$emit('update:name', $event.target.value)" />

                <label class="fieldset-label text-lg" for="owner">Propietario</label>
                <input type="text" id="owner" class="input w-full" placeholder="Propietario de la mascota"
                    :value="owner" @input="$emit('update:owner', $event.target.value)" />

                <label class="fieldset-label text-lg" for="email">Correo electrónico</label>
                <input type="email" id="email" class="input w-full" placeholder="mail@site.com"
                    :value="email" @input="$emit('update:email', $event.target.value)" />

                <label class="fieldset-label text-lg" for="date">Fecha</label>
                <input type="date" id="date" class="input w-full"
                    :value="date" @input="$emit('update:date', $event.target.value)" />

                <label class="fieldset-label text-lg" for="symptom">Síntomas</label>
                <textarea id="symptom" placeholder="Síntomas..." class="textarea w-full h-20" :value="symptom"
                    @input="$emit('update:symptom', $event.target.value)"></textarea>

                <button class="btn btn-primary mt-4 text-lg">{{ textButton }}</button>
            </fieldset>
        </form>
    </div>
</template>
