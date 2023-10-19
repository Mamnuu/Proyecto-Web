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
          >Formulario para el registro de clientes.</v-card-subtitle
        >
        <div class="text-subtitle-1 text-medium-emphasis">Nombre</div>

        <v-text-field
          v-model="nombre"
          density="compact"
          placeholder="Nombre"
          prepend-inner-icon="mdi-account-outline"
          variant="underlined"
          :rules="Rules"
        ></v-text-field>
        <div class="text-subtitle-1 text-medium-emphasis">Apellido</div>

        <v-text-field
          v-model="apellido"
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
          v-model="correo"
          density="compact"
          placeholder="Correo electrónico"
          prepend-inner-icon="mdi-email-outline"
          variant="underlined"
          :rules="Rules"
        ></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">Dirección</div>

        <v-text-field
          v-model="direccion"
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
          v-model="contacto"
          density="compact"
          placeholder="Número de contacto"
          prepend-inner-icon="mdi-card-account-phone-outline"
          variant="underlined"
          :rules= "[contactoRules, Rules].flat()"
        ></v-text-field>

        <v-btn
          block
          class="mb-8"
          color="#5995fd"
          size="large"
          variant="outlined"
          @click="register"
          >Registrar
        </v-btn>
      </v-card>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Swal from "sweetalert2";
let nextCustomerId = 8; // Contador para el ID secuencial
const img = "/images/perfil-del-usuario.png";
const cargo = "cliente";

const errorMessage = ref("");

export default {
  data: () => {
    return {
      Rules: [(v) => !!v || "El campo es obligatorio"],
      contactoRules: [v => (/^[0-9]+|[()\.]+/.test(v)) || 'Solo se permiten números'],
      nombre: "",
      apellido: "",
      correo: "",
      direccion: "",
      contacto: "",
    };
  },
  methods: {
    async getCustomers() {
      try {
        const response = await axios.get("http://localhost:3001/customers");
        this.customers = response.data;
      } catch (error) {
        console.error("Error al obtener clientes:", error);
      }
    },
    async register() {
      await this.getCustomers();

      // Validación campos vacíos
      if (
        this.nombre == "" ||
        this.apellido == "" ||
        this.correo == "" ||
        this.direccion == "" ||
        this.contacto == ""
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
      if (!(/^[0-9]+|[()\.]+/.test(this.contacto))) {
        errorMessage.value = "Solo se permiten números";
        Swal.fire({
          icon: "error",
          title: "Oops...",
          text: errorMessage.value,
        });
        return;
      }

      // Validación correo válido
      if (!this.correo || !/^\S+@\S+\.\S+$/.test(this.correo)) {
        console.log("Por favor, ingresa un correo electrónico válido.");
        errorMessage.value = "Por favor, ingresa un correo electrónico válido.";
        Swal.fire({
          icon: "error",
          title: "Oops...",
          text: errorMessage.value,
        });
        return;
      }

      // Verificar si existe el correo
      const emailExists = this.customers.some(
        (customer) => customer.correo === this.correo
      );

      if (emailExists) {
        console.error("El correo electrónico ya está registrado.");
      } else {
        // Generación de ID secuencial única para el proveedor
        const customerId = nextCustomerId;

        // Incrementa el contador para el siguiente proveedor
        nextCustomerId++;

        // Registra al proveedor con el ID generado
        const newCustomer = {
          id: customerId,
          nombre: this.nombre,
          apellido: this.apellido,
          correo: this.correo,
          direccion: this.direccion,
          contacto: this.contacto,
          img: img,
          cargo: cargo,
        };

        // Añade el proveedor al servidor
        await this.addCustomer(newCustomer);

        console.log("", newCustomer);
      }
    },
    async addCustomer(customer) {
      try {
        const response = await axios.post(
          "http://localhost:3001/customers", customer);
        console.log("Cliente agregado:", response.data);
        // Redirección al home
        this.$router.push("/clientes");
        Swal.fire({
          icon: "success",
          title: "Registro exitoso",
        });
      } catch (error) {
        console.error("Error al agregar cliente:", error);
        Swal.fire({
          icon: "error",
          title: "Error en el registro:",
        });
      }
    },
  },
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
  overflow:visible;
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
