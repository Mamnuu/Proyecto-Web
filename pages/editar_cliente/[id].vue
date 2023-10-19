<template>
  <div class="register-container">
    <div class="form-container">
      <v-card class="mx-auto pa-6 pb-2" elevation="20" max-width="448" rounded="lg">
        <v-card-title style="text-align: center">Editar a {{ foundCustomer.nombre }}</v-card-title>
        <v-card-subtitle style="text-align: center">Formulario para el registro de clientes.</v-card-subtitle>
        <div class="text-subtitle-1 text-medium-emphasis">Nombre</div>

        <v-text-field v-model="foundCustomer.nombre" density="compact" placeholder="Nombre"
          prepend-inner-icon="mdi-account-outline" variant="underlined" :rules="Rules"></v-text-field>
        <div class="text-subtitle-1 text-medium-emphasis">Apellido</div>

        <v-text-field v-model="foundCustomer.apellido" density="compact" placeholder="Apellido"
          prepend-inner-icon="mdi-account-outline" variant="underlined" :rules="Rules"></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">
          Correo electrónico
        </div>

        <v-text-field v-model="foundCustomer.correo" density="compact" placeholder="Correo electrónico"
          prepend-inner-icon="mdi-email-outline" variant="underlined" :rules="Rules"></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">Dirección</div>

        <v-text-field v-model="foundCustomer.direccion" density="compact" placeholder="Dirección"
          prepend-inner-icon="mdi-directions" variant="underlined" :rules="Rules"></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">
          Número de contacto
        </div>

        <v-text-field v-model="foundCustomer.contacto" density="compact" placeholder="Número de contacto"
          prepend-inner-icon="mdi-card-account-phone-outline" variant="underlined"
          :rules="[contactoRules, Rules].flat()"></v-text-field>

        <v-btn block class="mb-8" color="#5995fd" size="large" variant="outlined" @click="update">Actualizar
        </v-btn>
      </v-card>
    </div>
  </div>
</template>
<script setup>
import axios from "axios";
import { useRoute } from "vue-router";
const customers = ref([]);
const route = useRoute();
const router = useRouter();
const customerId = Number(route.params.id); // Asegúrate de que el id sea de tipo número
let foundCustomer = ref({});
const errorMessage = ref("");

const Rules = [(v) => !!v || "Este campo es requerido"];
const contactoRules = [v => (/^[-+]?[0-9]*\.?[0-9]*$/.test(v)) || 'Solo se permiten números'];

onBeforeMount(async () => {
  try {
    await getCustomers();
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

const update = async () => {
  try {
    await updateCustomers();
    console.log("Cliente actualizado correctamente");
    router.push({
      path: `/home`,
    });
    Swal.fire({
      icon: "success",
      title: "Cliente actualizado correctamente",
      showConfirmButton: false,
      timer: 1500,
    });
  } catch (error) {
    console.log(errorMessage);
  }
}

const updateCustomers = async () => {
  const url = `http://localhost:3001/customers/${foundCustomer.value.id}`;
  const response = await axios.put(url, foundCustomer.value);
  console.log(response);
  costumers.value = response.data;
};

definePageMeta({
  layout: "blank",
})
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
