<template>
  <v-container class="container-p">
    <h3 class="titulo">Productos</h3>
    <br />
    <v-row>
      <v-col
        v-for="producto in productos"
        :key="producto.id"
        cols="12"
        md="4"
        sm="6"
      >
        <v-card class="productoCard" max-width="344" elevation="3">
          <v-img :src="producto.img" height="200px" cover></v-img>

          <v-card-title>
            {{ producto.nombre }}
          </v-card-title>

          <v-card-subtitle>
            <v-icon> mdi-currency-usd </v-icon>
            {{ producto.precio }}
          </v-card-subtitle>

          <v-card-actions>
            <v-btn class="btnCard" variant="text"> Ver producto </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
      <v-btn fab dark large color=#5995fd class="btn-flotante">
        <v-icon size="50">mdi-plus</v-icon>
      </v-btn>
    </v-row>
  </v-container>
</template>
<script setup>
import Swal from "sweetalert2";
import axios from "axios";
const productos = ref([]);

const getProducts = async () => {
  const url = "http://localhost:3001/products";
  const response = await axios.get(url);
  productos.value = response.data;
};
getProducts();

//Alerta de inicio de sesion existoso
const Toast = Swal.mixin({
  toast: true,
  position: "top-end",
  showConfirmButton: false,
  timer: 3000,
  timerProgressBar: true,
  didOpen: (toast) => {
    toast.addEventListener("mouseenter", Swal.stopTimer);
    toast.addEventListener("mouseleave", Swal.resumeTimer);
  },
});

Toast.fire({
  icon: "success",
  title: "Inicio de sesión exitoso",
});
</script>

<style>
.titulo {
  font-family: sans-serif;
  font-weight: bold;
  text-align: center;
  font-size: 30px !important;
}

.btnCard {
  color: #5995fd !important;
}

.productoCard {
  background-color: whitesmoke;
  color: black;
  border-radius: 15px !important;
}
.btn-flotante {
  size: 50px !important;
  color: #ffffff; /* Color del texto */
  border-radius: 50px !important; /* Borde del boton */
  position: fixed !important;
  bottom: 20px !important;
  right: 40px;
  box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.1);
  margin-left: 80% !important;
  margin-top: 100px !important;
  width: 80px !important;
  height: 80px !important;
  background-color: #2c2fa5;
}
.btn-flotante:hover {
  background-color: #2c2fa5; /* Color de fondo al pasar el cursor */
  box-shadow: 0px 15px 20px rgba(0, 0, 0, 0.3);
  transform: translateY(-7px);
}
@media only screen and (max-width: 600px) {
  .btn-flotante {
    padding: 12px 20px;
    bottom: 20px;
    right: 20px;
  }
}
</style>
