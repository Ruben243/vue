<script setup>
  defineEmits(["actualizar-cita", "eliminar-cita"]);

  defineProps({
    cita: {
      type: Object,
      required: true,
    },
  });

  const horaFin = (cita) => {
    const fechaNormal = new Date(cita);
    // Obtener los componentes de la fecha y hora
    const fechaHora = new Date(fechaNormal);
    const diaSemana = fechaHora.toLocaleDateString("es-ES", {
      weekday: "long",
    });
    const dia = fechaHora.getDate();
    const mes = fechaHora.toLocaleDateString("es-ES", { month: "long" });
    const hora = fechaHora.getHours();
    const minutos = fechaHora.getMinutes().toString().padStart(2, "0");
    // Construir la cadena formateada
    const cadenaFormateada = `${diaSemana} ${dia} de ${mes} a las ${hora}:${minutos}`;
    return cadenaFormateada;
  };
</script>

<template>
  <div class="mx-5 my-10 bg-white shadow-md px-5 py-10 rounded-xl">
    <p class="font-bold mb-3 text-gray-700 uppercase">
      ID:
      <span class="font-normal normal-case">
        {{ cita.id }}
      </span>
    </p>

    <p class="font-bold mb-3 text-gray-700 uppercase">
      Contacto:
      <span class="font-normal normal-case">
        {{ cita.contacto }}
      </span>
    </p>

    <p class="font-bold mb-3 text-gray-700 uppercase">
      Lugar:
      <span class="font-normal normal-case">
        {{ cita.lugar }}
      </span>
    </p>

    <p class="font-bold mb-3 text-gray-700 uppercase">
      Email:
      <span class="font-normal normal-case">
        {{ cita.email }}
      </span>
    </p>

    <p class="font-bold mb-3 text-gray-700 uppercase">
      Fecha:
      <span
        v-text="horaFin(cita.fecha)"
        class="font-normal normal-case">
      </span>
    </p>

    <p class="font-bold mb-3 text-gray-700 uppercase">
      Indicaciones:
      <span class="font-normal normal-case">
        {{ cita.observaciones }}
      </span>
    </p>

    <div class="grid md:grid-cols-2 gap-5 mt-10">
      <button
        type="button"
        @click="$emit('actualizar-cita', cita.id)"
        class="block w-full py-2 px-10 bg-indigo-600 hover:bg-indigo-700 text-white font-bold uppercase rounded-lg">
        Editar
      </button>

      <button
        type="button"
        @click="$emit('eliminar-cita', cita.id)"
        class="block w-full py-2 px-10 bg-red-600 hover:bg-red-700 text-white font-bold uppercase rounded-lg">
        Eliminar
      </button>
    </div>
  </div>
</template>
