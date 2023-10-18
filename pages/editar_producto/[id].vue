<template>
  <h1 v-if="foundProduct">{{ foundProduct.nombre }}</h1>
</template>
<script setup>
import axios from "axios";
import { useRoute } from "vue-router";
const products = ref([]);
const route = useRoute();
const productId = Number(route.params.id); // Asegúrate de que el id sea de tipo número

let foundProduct = ref(null);

onBeforeMount(async () => {
  try {
    await getProducts();
    console.log(products.value);
    foundProduct.value = products.value.find((product) => product.id == productId);
    if (!foundProduct) {
      throw new Error("Producto no encontrado");
    }
    console.log(foundProduct.value);
  } catch (error) {
    console.error("Error al encontrar el producto:", error);
  }
});

const getProducts = async () => {
  const url = "http://localhost:3001/products";
  const response = await axios.get(url);
  products.value = response.data;
};
</script>