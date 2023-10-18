<template>
  <h1 v-if="foundCustomer">{{ foundCustomer.nombre }}</h1>
</template>
<script setup>
import axios from "axios";
import { useRoute } from "vue-router";
const customers = ref([]);
const route = useRoute();
const customerId = Number(route.params.id); // Asegúrate de que el id sea de tipo número

let foundCustomer = ref(null);

onBeforeMount(async () => {
  await getCustomers();
  try {
    console.log(customers.value);
    foundCustomer.value = customers.value.find((customer) => customer.id == customerId);
    if (!foundCustomer) {
      throw new Error("Cliente no encontrado");
    }
    console.log(foundCustomer.value);
  } catch (error) {
    console.error("Error al encontrar el cliente:", error);
  }
});

const getCustomers = async () => {
  const url = "http://localhost:3001/customers";
  const response = await axios.get(url);
  customers.value = response.data;
};
</script>