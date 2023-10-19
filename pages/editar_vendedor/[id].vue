<template>
  <div class="register-container">
    <div class="form-container">
      <v-card class="mx-auto pa-6 pb-2" elevation="20" max-width="448" rounded="lg">
        <v-card-title style="text-align: center">Vendedores</v-card-title>
        <v-card-subtitle style="text-align: center">Formulario para actualizar vendedores</v-card-subtitle>
        <br>
        <div class="text-subtitle-1 text-medium-emphasis">Nombres</div>

        <v-text-field v-model="foundSeller.nombre" density="compact" placeholder="Nombres"
          prepend-inner-icon="mdi-account-outline" variant="underlined" :rules="Rules"></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">Apellidos</div>

        <v-text-field v-model="foundSeller.apellido" density="compact" placeholder="Apellidos"
          prepend-inner-icon="mdi-account-outline" variant="underlined" :rules="Rules"></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">
          Correo electrónico
        </div>

        <v-text-field v-model="foundSeller.correo" density="compact" placeholder="Correo electrónico"
          prepend-inner-icon="mdi-email-outline" variant="underlined" :rules="Rules"></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">
          Número de contacto
        </div>

        <v-text-field v-model="foundSeller.contacto" density="compact" placeholder="Número de contacto"
          prepend-inner-icon="mdi-card-account-phone-outline" variant="underlined"
          :rules="[Rules, contactoRules].flat()"></v-text-field>


        <v-btn block class="mb-8" color="#5995fd" size="large" variant="outlined" @click="update">
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
const sellers = ref([]);
const route = useRoute();
const router = useRouter();
const sellerId = Number(route.params.id); // Asegúrate de que el id sea de tipo número
let foundSeller = ref({});
const errorMessage = ref("");


const Rules = [(v) => !!v || "Este campo es requerido"];
const contactoRules = [v => (/^[-+]?[0-9]*\.?[0-9]*$/.test(v)) || 'Solo se permiten números'];

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

const update = async () => {
  try {
    await updateSellers();
    console.log("Vendedor actualizado correctamente");
    router.push({
      path: `/vendedores`,
    });
    Swal.fire({
      icon: "success",
      title: "Vendedor actualizado correctamente",
      showConfirmButton: false,
      timer: 1500,
    });
  } catch (error) {
    console.log(errorMessage);
  }
}

const updateSellers = async () => {
  const url = `http://localhost:3001/sellers/${foundSeller.value.id}`;
  const response = await axios.put(url, foundSeller.value);
  console.log(response);
  sellers.value = response.data;
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
  overflow: hidden;
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
