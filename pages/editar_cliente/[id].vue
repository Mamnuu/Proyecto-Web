<template>
  <div class="register-container">
    <div class="form-container">
      <v-card
        class="mx-auto pa-6 pb-2"
        elevation="20"
        max-width="448"
        rounded="lg"
      >
        <v-card-title style="text-align: center">Clientes</v-card-title>
        <v-card-subtitle style="text-align: center"
          >Formulario para actualizar clientes</v-card-subtitle
        >
        <div class="text-subtitle-1 text-medium-emphasis">Nombre</div>

        <v-text-field
          v-model="foundCustomer.name"
          density="compact"
          placeholder="Nombre"
          prepend-inner-icon="mdi-account-outline"
          variant="underlined"
          :rules="Rules"
        ></v-text-field>
        <div class="text-subtitle-1 text-medium-emphasis">Apellido</div>

        <v-text-field
          v-model="foundCustomer.lastname"
          density="compact"
          placeholder="Apellido"
          prepend-inner-icon="mdi-account-outline"
          variant="underlined"
          :rules="Rules"
        ></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">
          Correo electrónico
        </div>

        <v-text-field
          v-model="foundCustomer.email"
          density="compact"
          placeholder="Correo electrónico"
          prepend-inner-icon="mdi-email-outline"
          variant="underlined"
          :rules="Rules"
        ></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">Dirección</div>

        <v-text-field
          v-model="foundCustomer.address"
          density="compact"
          placeholder="Dirección"
          prepend-inner-icon="mdi-directions"
          variant="underlined"
          :rules="Rules"
        ></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">
          Número de contacto
        </div>

        <v-text-field
          v-model="foundCustomer.contact"
          density="compact"
          placeholder="Número de contacto"
          prepend-inner-icon="mdi-card-account-phone-outline"
          variant="underlined"
          :rules="[contactoRules, Rules].flat()"
        ></v-text-field>

        <v-btn
          block
          class="mb-8"
          color="#5995fd"
          size="large"
          variant="outlined"
          @click="validatefields"
          >Actualizar
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

const customers = ref([]);
const route = useRoute();
const router = useRouter();
const customerId = route.params.id; // Asegúrate de que el id sea de tipo número
let foundCustomer = ref({});
const errorMessage = ref("");

const Rules = [(v) => !!v || "Este campo es requerido"];
const contactoRules = [
  (v) => /^[-+]?[0-9]*\.?[0-9]*$/.test(v) || "Solo se permiten números",
];

onBeforeMount(async () => {
  try {
    await getCustomers();
    console.log(customers.value);
    foundCustomer.value = customers.value.find(
      (customer) => customer._id == customerId
    );
    if (!foundCustomer) {
      throw new Error("Cliente no encontrado");
    }
    console.log(foundCustomer.value);
  } catch (error) {
    console.error("Error al encontrar el cliente:", error);
  }
});

const getCustomers = async () => {
  const url = `${config.api_host}/customers`;
  const token = localStorage.getItem("token")
  const headers = getHeaders(token);
  const { data } = await axios.get(url, { headers });
  customers.value = data.info
};
const validatefields = async () => {
  if (
    foundCustomer.value.name == "" ||
    foundCustomer.value.lastname == "" ||
    foundCustomer.value.email == "" ||
    foundCustomer.value.address == "" ||
    foundCustomer.value.contact == ""
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
  if (!/[0-9-]+/.test(foundCustomer.value.contact)) {
    errorMessage.value = "Solo se permiten números";
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: errorMessage.value,
    });
    return;
  }

  // Validación correo válido
  if (!foundCustomer.value.email || !/^\S+@\S+\.\S+$/.test(foundCustomer.value.email)) {
    console.log("Por favor, ingresa un correo electrónico válido.");
    errorMessage.value = "Por favor, ingresa un correo electrónico válido.";
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
    await updateCustomers();
    console.log("Cliente actualizado correctamente");
    router.push({
      path: `/clientes`,
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
};

const updateCustomers = async () => {
  console.log(foundCustomer.value);
  const url = `${config.api_host}/customers/${foundCustomer.value._id}`;
  const token = localStorage.getItem("token");
  const headers = getHeaders(token);
  const response = await axios.put(url, foundCustomer.value, { headers });
  console.log(response);
  customers.value = response.data;
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
