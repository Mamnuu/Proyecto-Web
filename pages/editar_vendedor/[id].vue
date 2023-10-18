<template>
    <h1 v-if="foundSeller">{{ foundSeller.nombre }}</h1>
  </template>
  <script setup>
  import axios from "axios";
  import { useRoute } from "vue-router";
  const sellers = ref([]);
  const route = useRoute();
  const sellerId = Number(route.params.id); // Asegúrate de que el id sea de tipo número
  
  let foundSeller = ref(null);
  
  onBeforeMount(async () => {
    try {
      await getSellers();
      console.log(sellers.value);
      foundSeller.value = sellers.value.find((seller) => seller.id == sellerId);
      if (!foundSeller) {
        throw new Error("Vendedor no encontrado");
      }
      console.log(foundSeller.value);
    } catch (error) {
      console.error("Error al encontrar el vendedor:", error);
    }
  });
  
  const getSellers = async () => {
    const url = "http://localhost:3001/sellers";
    const response = await axios.get(url);
    sellers.value = response.data;
  };
  </script>