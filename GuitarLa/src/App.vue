<script setup>
  import { ref, onMounted, watch } from "vue";
  import { db } from "./data/guitarras";
  import Guitarra from "./components/Guitarra.vue";
  import Header from "./components/Header.vue";
  import Footer from "./components/Footer.vue";

  const guitarras = ref([]);
  const carrito = ref([]);
  const guitarra = ref({});

watch(carrito, () => {
  guardarLocalStorage();
}, {
    deep:true,
  });

  onMounted(() => {
    guitarras.value = db;
    guitarra.value = db[3];
    const carritolStorage = localStorage.getItem("carrito");
    if (carritolStorage) {
      carrito.value = JSON.parse(carritolStorage);
    }
  });

  const guardarLocalStorage = () => {
    localStorage.setItem("carrito", JSON.stringify(carrito.value));
  };

  //Method Handler
  const incrementarCarrito = (guitarra) => {
    const existeCarrito = carrito.value.findIndex(
      (producto) => producto.id === guitarra.id
    );

    if (existeCarrito >= 0) {
      carrito.value[existeCarrito].cantidad++;
    } else {
      guitarra.cantidad = 1;
      carrito.value.push(guitarra);
    }
  };

  const restarCantidad = (id) => {
    const index = carrito.value.findIndex((producto) => producto.id === id);
    if (carrito.value[index].cantidad <= 1) return;

    carrito.value[index].cantidad--;
    
  };
  const sumarCantidad = (id) => {
    const index = carrito.value.findIndex((producto) => producto.id === id);
    carrito.value[index].cantidad++;
  };

  const eliminarProducto = (id) => {
    carrito.value = carrito.value.filter((producto) => producto.id !== id);
  };
  const vaciarCarrito = () => {
    carrito.value = [];
  };
</script>

<template>
  <Header
    :carrito="carrito"
    :guitarra="guitarra"
    @sumar-cantidad="sumarCantidad"
    @restar-cantidad="restarCantidad"
    @agregar-carrito="incrementarCarrito"
    @eliminar-producto="eliminarProducto"
    @vaciar-carrito="vaciarCarrito" />

  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <Guitarra
        v-for="guitarra in guitarras"
        :key="guitarra.id"
        :guitarra="guitarra"
        @incrementar-carrito="incrementarCarrito" />
      <!-- FIN GUITARRA -->
    </div>
  </main>

  <Footer />
</template>
