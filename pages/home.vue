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
            {{ producto.name }}
          </v-card-title>

          <v-card-subtitle>
            {{ formatToCOP(parseFloat(producto.price)) }}
          </v-card-subtitle>

          <v-card-actions>
            <v-btn
              class="btnCard"
              variant="text"
              block
              @click="openDialog(producto)"
            >
              Ver producto
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
      <v-card v-if="dialog" class="info" elevation="7">
        <v-dialog
          v-model="dialog"
          width="auto"
          style="background: none !important; border-radius: 80px !important"
        >
          <DescripcionP
            :producto="currentProduct"
            :dialog="dialog"
            @closeDialog="closeDialog"
          ></DescripcionP>
          <v-btn class="btnCerrar" @click="dialog = false" icon elevation="0"
            ><v-icon class="iconC">mdi-close</v-icon></v-btn
          >
        </v-dialog>
      </v-card>
      <v-btn
        fab
        dark
        large
        color="#5995fd"
        class="btn-flotante"
        to="/crearProducto"
      >
        <v-icon size="50">mdi-plus</v-icon>
      </v-btn>
    </v-row>
  </v-container>
</template>
<script setup>
import Swal from "sweetalert2";
import axios from "axios";
import DescripcionP from "~/components/producto.vue";
import config from "~/config/default.json";
import { getHeaders } from "~/src/auth/jwt.js";

const productos = ref([]);
const dialog = ref(false);
const currentProduct = ref(null);

const formatToCOP = (value) => {
    return value.toLocaleString('es-CO', { style: 'currency', currency: 'COP' });
  }

const getProducts = async () => {
  const url = `${config.api_host}/products`;
  const token = localStorage.getItem("token")
  const headers = getHeaders(token);
  const { data } = await axios.get(url, { headers });
  productos.value = data.info;
};

onBeforeMount(() => {
  getProducts();
});

const closeDialog = () => {
  dialog.value = false;
};
//Escuchador para el evento emitido desde el componente `productos.vue`
watch(dialog, (newValue) => {
  if (!newValue) {
    dialog.value = false;
  }
});

//Alerta de inicio de sesion existoso

let logged_user = sessionStorage.getItem("LOGGEDUSER");
if (logged_user != "true") {
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
  sessionStorage.setItem("LOGGEDUSER", "true");
}

const openDialog = (producto) => {
  currentProduct.value = producto;
  dialog.value = true;
};
</script>

<style>
.titulo {
  font-family: sans-serif;
  font-weight: bold;
  text-align: center;
  font-size: 30px !important;
}

.btnCerrar {
  position: absolute;
  top: -3%;
  right: 1%;
  outline: none !important;
  border: 0 !important;
}

.iconC {
  font-size: 30px !important;
}

.info {
  border-radius: 40px !important;
}

.btnCard {
  color: #5995fd !important;
}

.btnSalir {
  background-color: #5995fd !important;
  color: white !important;
  font-weight: bold !important;
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
