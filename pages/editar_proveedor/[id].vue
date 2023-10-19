<template>
  <div class="register-container">
    <div class="form-container">
      <v-card
        class="mx-auto pa-6 pb-2"
        elevation="20"
        max-width="448"
        rounded="lg"
      >
        <v-card-title style="text-align: center">Proveedores</v-card-title>
        <v-card-subtitle style="text-align: center"
          >Formulario para actualizar proveedores</v-card-subtitle
        >
        <div class="text-subtitle-1 text-medium-emphasis">Nombre</div>

        <v-text-field
          v-model="foundSupplier.nombre"
          density="compact"
          placeholder="Nombre"
          prepend-inner-icon="mdi-account-outline"
          variant="underlined"
          :rules="Rules"
        ></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">
          Correo electrónico
        </div>

        <v-text-field
          v-model="foundSupplier.correo"
          density="compact"
          placeholder="Correo electrónico"
          prepend-inner-icon="mdi-email-outline"
          variant="underlined"
          :rules="Rules"
        ></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">
          Producto que provee
        </div>

        <v-text-field
          v-model="foundSupplier.producto"
          density="compact"
          placeholder="Producto"
          prepend-inner-icon="mdi-cart-outline"
          variant="underlined"
          :rules="Rules"
        ></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">
          Número de contacto
        </div>

        <v-text-field
          v-model="foundSupplier.contacto"
          density="compact"
          placeholder="Número de contacto"
          prepend-inner-icon="mdi-card-account-phone-outline"
          variant="underlined"
          :rules="[Rules, contactoRules].flat()"
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
const suppliers = ref([]);
const route = useRoute();
const router = useRouter();
const supplierId = Number(route.params.id); // Asegúrate de que el id sea de tipo número
let foundSupplier = ref({});
const errorMessage = ref("");

const Rules = [(v) => !!v || "Este campo es requerido"];
const contactoRules = [
  (v) => /^[0-9]+|[()\.]+/.test(v) || "Solo se permiten números",
];

onBeforeMount(async () => {
  try {
    await getSuppliers();
    console.log(suppliers.value);
    foundSupplier.value = suppliers.value.find(
      (supplier) => supplier.id == supplierId
    );
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
const validatefields = async () => {
  if (
    foundSupplier.value.nombre == "" ||
    foundSupplier.value.correo == "" ||
    foundSupplier.value.producto == "" ||
    foundSupplier.value.contacto == ""
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
  if (!/[0-9-]+/.test(foundSupplier.value.contacto)) {
    errorMessage.value = "Solo se permiten números";
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: errorMessage.value,
    });
    return;
  }

  // Validación correo válido
  if (!foundSupplier.value.correo || !/^\S+@\S+\.\S+$/.test(foundSupplier.value.correo)) {
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
    await updateSuppliers();
    console.log("Proveedor actualizado correctamente");
    router.push({
      path: `/proveedores`,
    });
    Swal.fire({
      icon: "success",
      title: "Proveedor actualizado correctamente",
      showConfirmButton: false,
      timer: 1500,
    });
  } catch (error) {
    console.log(errorMessage);
  }
};

const updateSuppliers = async () => {
  const url = `http://localhost:3001/suppliers/${foundSupplier.value.id}`;
  const response = await axios.put(url, foundSupplier.value);
  console.log(response);
  suppliers.value = response.data;
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
