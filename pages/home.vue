<template>
    <v-container class="container-p">
    <h3 class="titulo">Productos</h3>
    <br>
    <v-row>
      <v-col v-for="producto in productos" :key="producto.id" cols="12" md="4" sm="6"  >
        <v-card class="productoCard" max-width="344" elevation="3">
        <v-img
        src="./images/imgChorizos.jpg"
        height="200px"
        cover
        ></v-img>

        <v-card-title>
        {{producto.nombre}}
        </v-card-title>

        <v-card-subtitle>
        <v-icon>
            mdi-currency-usd
        </v-icon>
        {{producto.precio}}
        </v-card-subtitle>

        <v-card-actions>
        <v-btn
            class="btnCard"
            variant="text"
        >
            Ver producto
        </v-btn>
        </v-card-actions>
    </v-card>
</v-col>
</v-row>
  
</v-container>
</template>
<script setup>
import Swal from "sweetalert2";
import axios from "axios";
const productos = ref([])

const getProducts = async () => {
    const url = "http://localhost:3001/products"
    const response = await axios.get(url)
    productos.value = response.data
}
getProducts();

//Alerta de inicio de sesion existoso
const Toast = Swal.mixin({
  toast: true,
  position: 'top-end',
  showConfirmButton: false,
  timer: 3000,
  timerProgressBar: true,
  didOpen: (toast) => {
    toast.addEventListener('mouseenter', Swal.stopTimer)
    toast.addEventListener('mouseleave', Swal.resumeTimer)
  }
})

Toast.fire({
  icon: 'success',
  title: 'Inicio de sesión exitoso'
})
</script>

<style>
.titulo {
  font-family: sans-serif;
  font-weight: bold;
  text-align: center;
}

.btnCard{
    color:  rgba(35, 166, 241, 0.904)  !important;
    
}

.productoCard{
  background-color: whitesmoke;
  color: black;
  border-radius: 15px !important;
}

</style>