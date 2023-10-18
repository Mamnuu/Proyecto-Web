<template>
    <h1 v-if="foundSupplier">{{ foundSupplier.nombre }}</h1>
  </template>
  <script setup>
  import axios from "axios";
  import { useRoute } from "vue-router";
  const suppliers = ref([]);
  const route = useRoute();
  const supplierId = Number(route.params.id); // Asegúrate de que el id sea de tipo número
  
  let foundSupplier = ref(null);
  
  onBeforeMount(async () => {
    try {
      await getSuppliers();
      console.log(suppliers.value);
      foundSupplier.value = suppliers.value.find((supplier) => supplier.id == supplierId);
      if (!foundSupplier) {
        throw new Error("Proveedor no encontrado");
      }
      console.log(foundSupplier.value);
    } catch (error) {
      console.error("Error al encontrar el proveedor:", error);
    }
  });
  
  const getSuppliers = async () => {
    const url = "http://localhost:3001/suppliers";
    const response = await axios.get(url);
    suppliers.value = response.data;
  };
  </script>