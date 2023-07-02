<script setup>
  import { ref, reactive, watch, onMounted } from "vue";
  import { uid } from "uid";
  import Header from "./components/Header.vue";
  import Formulario from "./components/Formulario.vue";
  import Cita from "./components/Cita.vue";
  import Swal from "sweetalert2";

  const citas = ref([]);
  console.log(citas);
  const cita = reactive({
    id: null,
    contacto: "",
    lugar: "",
    fecha: "",
    email: "",
    observaciones: "",
  });

  const guardarCita = () => {
    if (cita.id) {
      const { id } = cita;
      const i = citas.value.findIndex((citaState) => citaState.id == id);

      citas.value[i] = { ...cita };

      Swal.fire({
        title: "Exito",
        text: "Registro actualizado correctamente",
        icon: "success",
        timer: 1500,
        showConfirmButton: false,
      });
    } else {
      //hace una copia no reactiva
      if (citas.value === null) {
        citas.value = []; // Asignar un array vacío si citas.value es null
      }
      citas.value.push({
        ...cita,
        id: uid(),
      });

      Swal.fire({
        title: "Exito",
        text: "Registro creado correctamente",
        icon: "success",
        timer: 1500,
        showConfirmButton: false,
      });
    }

    //borra el formulario
    Object.assign(cita, {
      contacto: "",
      lugar: "",
      fecha: "",
      email: "",
      observaciones: "",
      id: null,
    });
  };

  // PERSINTENCIA
  watch(
    citas,
    () => {
      persistencia();
    },
    {
      deep: true,
    }
  );
  const persistencia = () => {
    localStorage.setItem("citas", JSON.stringify(citas.value));
  };

  onMounted(() => {
    const citasStorage = localStorage.getItem("citas");
    if (citasStorage !== "undefined") {
      citas.value = JSON.parse(citasStorage);
    }
  });

  //ACTUALIZAR
  const actualizarCita = (id) => {
    const citaEditar = citas.value.filter((cita) => cita.id === id)[0];
    Object.assign(cita, citaEditar);
  };

  const eliminarCita = (id) => {
    citas.value = citas.value.filter((cita) => cita.id !== id);
    Swal.fire({
      title: "Exito",
      text: "Registro Eliminado correctamente",
      icon: "success",
      timer: 1500,
      showConfirmButton: false,
    });
  };
</script>

<template>
  <div class="contenedor">
    <Header />
    <div class="formulario">
      <Formulario
        v-model:contacto="cita.contacto"
        v-model:lugar="cita.lugar"
        v-model:fecha="cita.fecha"
        v-model:email="cita.email"
        v-model:observaciones="cita.observaciones"
        @guardar-cita="guardarCita"
        :id="cita.id" />

      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">Administra tus citas</h3>
        <div v-if="citas?.length > 0 || citas != null">
          <p class="formulario_p">
            Informacion sobre tu
            <span class="text-indigo-600 font-bold">Citas</span>
          </p>
          <Cita
            v-for="cita in citas"
            :cita="cita"
            @actualizar-cita="actualizarCita"
            @eliminar-cita="eliminarCita" />
        </div>
        <p
          v-else
          class="mt-10 text-2xl text-center">
          No tienes ninguna cita
        </p>
      </div>
    </div>
  </div>
</template>
