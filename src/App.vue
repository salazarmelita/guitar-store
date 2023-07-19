<!-- Sólo agregando <script> ya se está trabajando con Composition API -->
<script setup>
import { ref, reactive, onMounted } from 'vue'
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';
import Guitarra from './components/Guitarra.vue';
import { db } from './data/guitarras'

// const state = reactive({
//  guitarras: db
// }) 

const guitarras = ref([]);
const carrito = ref([])


// Componente de ciclo de vida
onMounted(() => {
    guitarras.value = db
    // state.guitarras = db
});

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
</script>

<template>
    <Header 
        :carrito="carrito"    
    />
    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>
        <div class="row mt-5">
            <Guitarra v-for="guitarra in guitarras" v-bind:guitarra="guitarra" @agregar-carrito="agregarCarrito" />
        </div>
    </main>
    <Footer />
</template>


<style scoped></style>
