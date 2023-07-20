<!-- Sólo agregando <script> ya se está trabajando con Composition API -->
<script setup>
import { ref, reactive, onMounted, watch } from 'vue'
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';
import Guitarra from './components/Guitarra.vue';
import { db } from './data/guitarras'

// const state = reactive({
//  guitarras: db
// }) 

const guitarras = ref([]);
const carrito = ref([])
const guitarra = ref({})


// Componente de ciclo de vida
onMounted(() => {
    guitarras.value = db
    guitarra.value = db[3]
    // state.guitarras = db
    const carritoStorage = localStorage.getItem('carrito')
    if(carritoStorage) {
        carrito.value = JSON.parse(carritoStorage)
    }
});

watch(carrito, ()=>{
    guardarLocalStorage();
}, {
    deep: true
})

const guardarLocalStorage = () => {
    localStorage.setItem('carrito', JSON.stringify(carrito.value))
}

const agregarCarrito = (guitarra) => {
    // Comprobar si un elemento existe en el carrito:
    const existeCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id)
    if (existeCarrito >= 0) {
        carrito.value[existeCarrito].cantidad++
    } else {
        guitarra.cantidad = 1;
        carrito.value.push(guitarra)
    }
};

const decrementarCantidad = (id) => {
    const index = carrito.value.findIndex(producto => producto.id === id)
    if (carrito.value[index].cantidad <= 1) return
    carrito.value[index].cantidad--
}

const incrementarCantidad = (id) => {
    const index = carrito.value.findIndex(producto => producto.id === id)
    if (carrito.value[index].cantidad >= 5) return
    carrito.value[index].cantidad++
}

const eliminarProducto = (id) => {
    carrito.value = carrito.value.filter(producto => producto.id !== id)
}

const vaciarCarrito = (guitarra) => {
    carrito.value = []
}
</script>

<template>
    <Header :carrito="carrito" :guitarra="guitarra" @agregar-carrito="agregarCarrito" @decrementar-cantidad="decrementarCantidad" @incrementar-cantidad="incrementarCantidad" @eliminar-producto="eliminarProducto" @vaciar-carrito="vaciarCarrito"/>
    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>
        <div class="row mt-5">
            <Guitarra v-for="guitarra in guitarras" :key="guitarra.id" v-bind:guitarra="guitarra" @agregar-carrito="agregarCarrito" />
        </div>
    </main>
    <Footer />
</template>


<style scoped></style>
