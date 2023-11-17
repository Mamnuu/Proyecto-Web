<template>
  <div class="register-container">
    <div class="form-container">
      <v-card class="mx-auto pa-6 pb-2" elevation="20" max-width="448" rounded="lg">
        <v-card-title style="text-align: center">{{
          foundProduct.name
        }}</v-card-title>
        <v-card-subtitle style="text-align: center">Formulario para actualizar productos</v-card-subtitle>
        <br />
        <div class="text-subtitle-1 text-medium-emphasis">Nombre</div>

        <v-text-field v-model="foundProduct.name" density="compact" placeholder="Nombre"
          prepend-inner-icon="mdi-account-outline" variant="underlined" :rules="Rules"></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">Precio</div>

        <v-text-field v-model="foundProduct.price" density="compact" placeholder="Precio" prepend-inner-icon="mdi-cash"
          variant="underlined" :rules="[precioRules, Rules].flat()"></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">Descripción</div>
        <v-textarea name="input-7-4" v-model="foundProduct.description" density="compact"
          prepend-inner-icon="text-box-outline" :rules="Rules"></v-textarea>

        <v-btn block class="mb-8" color="#5995fd" size="large" variant="outlined" @click="validatefields">
          Actualizar
        </v-btn>
      </v-card>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { useRoute } from "vue-router";
import Swal from "sweetalert2";
import config from "~/config/default.json";
import { getHeaders } from "~/src/auth/jwt.js";
const products = ref([]);
const route = useRoute();
const router = useRouter();
const productId = route.params.id; // Asegurarse de que el id sea de tipo número
let foundProduct = ref({});
const errorMessage = ref("");

const Rules = [(v) => !!v || "Este campo es requerido"];
const precioRules = [
  (v) => /^[-+]?[0-9]*\.?[0-9]*$/.test(v) || "Solo se permiten números",
];

onBeforeMount(async () => {
  try {
    await getProducts();
    console.log(products.value);
    foundProduct.value = products.value.find(
      (product) => product._id == productId
    );
    if (!foundProduct) {
      throw new Error("Producto no encontrado");
    }
    console.log(foundProduct.value);
  } catch (error) {
    console.error("Error al encontrar el producto:", error);
  }
});

const getProducts = async () => {
  const url = `${config.api_host}/products`;
  const token = localStorage.getItem("token")
  const headers = getHeaders(token);
  const { data } = await axios.get(url, { headers });
  products.value = data.info;
};
const validatefields = async () => {
  if (
    foundProduct.value.name == "" ||
    foundProduct.value.price == "" ||
    foundProduct.value.description == ""
  ) {
    errorMessage.value = "Por favor, ingresa todos los campos.";
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: errorMessage.value,
    });
    return;
  }
  // Validación de solo números
  if (!/[0-9-]+/.test(foundProduct.value.price)) {
    errorMessage.value = "Solo se permiten números,";
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: errorMessage.value,
    });
    return;
  }
  await update();
};
const update = async () => {
  try {
    await updateProducts();
    console.log("Producto actualizado correctamente");
    router.push({
      path: `/home`,
    });
    Swal.fire({
      icon: "success",
      title: "Producto actualizado correctamente",
      showConfirmButton: false,
      timer: 1500,
    });
  } catch (error) {
    console.log(errorMessage);
  }
};

const updateProducts = async () => {
  console.log(foundProduct.value);
  const url = `${config.api_host}/products/${foundProduct.value._id}`;
  const token = localStorage.getItem("token");
  const headers = getHeaders(token);
  const response = await axios.put(url, foundProduct.value, { headers });
  console.log(response);
  products.value = response.data;
};

definePageMeta({
  layout: "blank",
});
</script>

<style scoped>
/* Centra el formulario verticalmente y elimina el espacio alrededor */
.form-container {
  justify-content: center;
  align-items: center;
  height: 80%;
  padding-top: 1%;
  padding-bottom: 2%;
}

.form-container v-card {
  width: 100%;
  /* Ajusta el ancho de la tarjeta al 100% del contenedor */
}

.register-container {
  position: relative;
  width: 100%;
  background-color: #fff;
  height: 100%;
  overflow: visible;
  margin-top: 3%;
}

.register-container:before {
  content: "";
  position: absolute;
  height: 2000px;
  width: 2000px;
  top: -10%;
  right: 48%;
  transform: translateY(-50%);
  background-image: linear-gradient(-45deg, #4481eb 0%, #04befe 100%);
  transition: 1.8s ease-in-out;
  border-radius: 50%;
  z-index: 0;
}
</style>
