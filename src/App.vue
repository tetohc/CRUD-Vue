<script setup>
import { ref, reactive, watch, onMounted } from 'vue';
import { uid } from 'uid';
import Swal from 'sweetalert2';

import Header from './components/Header.vue';
import Form from './components/Form.vue';
import Patient from './components/Patient.vue';

const patients = ref([]);

// watch the patients and save it to local storage
watch(patients, () => {
  // If the patients is not empty, save it to local storage; otherwise, remove it.
  if (patients.value.length > 0){
    localStorage.setItem('patients', JSON.stringify(patients.value));
  } else {
    localStorage.removeItem('patients');
  }
}, { deep: true });

onMounted(() => {
  // check if there is a patients in local storage
  const patientsInLocalStorage = localStorage.getItem('patients');
  if (patientsInLocalStorage) {
    patients.value = JSON.parse(patientsInLocalStorage);
  }
});

const patient = reactive({
  id: null,
  name: '',
  owner: '',
  email: '',
  date: '',
  symptom: ''
});

const resetForm = () => {
  Object.assign(patient, {
    name: '',
    owner: '',
    email: '',
    date: '',
    symptom: '',
    id: null
  });
}

const showMessageCreateUpdate = (message) => {
  Swal.fire({
    icon: "success",
    title: "Éxito",
    text: message,
    showConfirmButton: false,
    timer: 1500
  });
}

const saveData = () => {
  if (patient.id) {
    console.log('actualizando');
    const index = patients.value.findIndex((x => x.id === patient.id));
    patients.value[index] = {
      ...patient
    };
    showMessageCreateUpdate('Paciente actualizado correctamente.');
  } else {
    patients.value.push({
      ...patient,
      id: uid()
    });
    showMessageCreateUpdate('Paciente agregado correctamente.');
  }
  resetForm();
}

// update the form with the selected patient data
const updatePatient = (id) => {
  let model = patients.value.filter((x) => x.id === id)[0];
  Object.assign(patient, model);
  
  Swal.fire({
    icon: "info",
    title: "Preparado para actualizar",
    text: 'Los datos del paciente han sido cargados en el formulario.',
    showConfirmButton: true,
    confirmButtonText: "Aceptar",
    confirmButtonColor: "#13ecf3",
  });
}

const deletePatient = (id) => {
  Swal.fire({
    title: "¿Está seguro que quiere eliminar?",
    text: "Esta acción no se puede revertir!",
    icon: "warning",
    showCancelButton: true,
    confirmButtonColor: "#13ecf3",
    cancelButtonColor: "#d33",
    confirmButtonText: "Sí, ¡eliminar!",
    cancelButtonText: "No, cancelar",
  }).then((result) => {
    if (result.isConfirmed) {
      let index = patients.value.findIndex((x) => x.id === id);
      patients.value.splice(index, 1);
      Swal.fire({
        title: "Eliminado",
        text: "El paciente ha sido eliminado correctamente.",
        icon: "success",
        showConfirmButton: true,
        confirmButtonText: "Aceptar",
        confirmButtonColor: "#13ecf3",
      });
    }
  });
}

</script>

<template>
  <Header />
  <div class="container mx-auto">
    <div class="md:flex">
      <Form v-model:name="patient.name" v-model:owner="patient.owner" v-model:email="patient.email"
        v-model:date="patient.date" v-model:symptom="patient.symptom" @save-data="saveData" :id="patient.id" />

      <div class="md:w-1/2 md:h-screen overflow-y-scroll mb-4">
        <h3 class="text-xl text-center font-semibold">Listado de pacientes</h3>
        <div v-if="patients.length > 0">
          <Patient v-for="patient in patients" :patient="patient" @update-patient="updatePatient"
            @delete-patient="deletePatient" />
        </div>
        <div v-else>
          <p class="text-center mt-3 mb-1">No hay pacientes registrados</p>
          <img src="./assets/img/photo-cat.png" alt="No hay pacientes" class="mx-auto w-1/2 rounded-lg shadow-lg" />
        </div>
      </div>
    </div>
  </div>
</template>