<script setup>
  import { reactive, computed } from "vue";
  import Swal from "sweetalert2";

  const alerta = reactive({
    tipo: "",
    mensaje: "",
  });

  const emit = defineEmits([
    "update:contacto",
    "update:lugar",
    "update:fecha",
    "update:email",
    "update:observaciones",
    "guardar-cita",
  ]);

  const props = defineProps({
    id: {
      type: [String, null],
      required: true,
    },
    contacto: {
      type: String,
      required: true,
    },
    lugar: {
      type: String,
      required: true,
    },
    fecha: {
      type: String,
      required: true,
    },
    email: {
      type: String,
      required: true,
    },
    observaciones: {
      type: String,
      required: true,
    },
  });

  const validar = () => {
    if (Object.values(props).includes("")) {
      Swal.fire({
        title: "Error",
        text: "Todos los campos son obligatorios",
        icon: "error",
        timer: 1500,
        showConfirmButton: false,
      });
      return;
    }
    emit("guardar-cita");
  };

  const editando = computed(() => {
    return props.id;
  });
</script>

<template>
  <div class="formulario_template">
    <h2 class="formulario_h2">Agenda para Citas</h2>
    <p class="formulario_p">
      Añade y <span class="text-indigo-600 font-bold">Gestiona</span> tus Citas
      con tus Contactos
    </p>

    <form
      class="formulario_form"
      @submit.prevent="validar">
      <div class="mb-5">
        <label
          for="contacto"
          class="campoForm"
          >Nombre Contacto</label
        >
        <input
          class="inputForm"
          type="text"
          id="contacto"
          placeholder="Nombre del contacto"
          :value="contacto"
          @input="$emit('update:contacto', $event.target.value)" />
      </div>
      <div class="mb-5">
        <label
          for="lugar"
          class="campoForm"
          >Lugar Cita</label
        >
        <input
          class="inputForm"
          type="text"
          id="lugar"
          placeholder="Lugar de la cita"
          :value="lugar"
          @input="$emit('update:lugar', $event.target.value)" />
      </div>
      <div class="mb-5">
        <label
          for="fecha"
          class="campoForm"
          >Fecha Cita</label
        >
        <input
          class="inputForm"
          type="datetime-local"
          name="fecha"
          id="fecha"
          :value="fecha"
          @input="$emit('update:fecha', $event.target.value)" />
      </div>
      <div class="mb-5">
        <label
          for="email"
          class="campoForm"
          >Email de contacto</label
        >
        <input
          class="inputForm"
          type="email"
          id="email"
          placeholder="Email de aviso"
          :value="email"
          @input="$emit('update:email', $event.target.value)" />
      </div>
      <div class="mb-5">
        <label
          for="observaciones"
          class="campoForm"
          >Indicaciones</label
        >
        <textarea
          class="textAreaForm"
          name="observaciones"
          id="observaciones"
          placeholder=" Indicaciones de la Cita"
          :value="observaciones"
          @input="
            $emit('update:observaciones', $event.target.value)
          "></textarea>
      </div>
      <input
        class="btnForm"
        type="submit"
        :value="[editando ? 'Guardar Cambios' : 'Registrar Cita']" />
    </form>
  </div>
</template>
