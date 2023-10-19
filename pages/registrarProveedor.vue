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
          >Formulario para el registro de proveedores.</v-card-subtitle>
        <div class="text-subtitle-1 text-medium-emphasis">Nombre</div>

        <v-text-field
          v-model="nombre"
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
          v-model="correo"
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
          v-model="producto"
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
          v-model="contacto"
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
let nextSupplierId = 6; // Contador para el ID secuencial
const img = "/images/imgproveedor.jpeg";
const cargo = "proveedor";

const errorMessage = ref("");

export default {
  data: () => {
    return {
      Rules: [(v) => !!v || "El campo es obligatorio"],
      contactoRules: [v => (/^[0-9]+|[()\.]+/.test(v)) || 'Solo se permiten números'],
      nombre: "",
      correo: "",
      producto: "",
      contacto: "",
    };
  },
  methods: {
    async getSuppliers() {
      try {
        const response = await axios.get("http://localhost:3001/suppliers");
        this.suppliers = response.data;
      } catch (error) {
        console.error("Error al obtener proveedores:", error);
      }
    },
    async register() {
      await this.getSuppliers();

      // Validación campos vacíos
      if (
        this.nombre == "" ||
        this.correo == "" ||
        this.producto == "" ||
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
      const emailExists = this.suppliers.some(
        (supplier) => supplier.correo === this.correo
      );

      if (emailExists) {
        console.error("El correo electrónico ya está registrado.");
      } 
      else {
        // Generación de ID secuencial única para el proveedor
        const supplierId = nextSupplierId;

        // Incrementa el contador para el siguiente proveedor
        nextSupplierId++;

        // Registra al proveedor con el ID generado
        const newSupplier = {
          id: supplierId,
          nombre: this.nombre,
          correo: this.correo,
          producto: this.producto,
          contacto: this.contacto,
          img: img,
          cargo: cargo,
        };

        // Añade el proveedor al servidor
        await this.addSupplier(newSupplier);

        console.log("", newSupplier);
      }
    },
    async addSupplier(supplier) {
      try {
        const response = await axios.post(
          "http://localhost:3001/suppliers",
          supplier
        );
        console.log("Proveedor agregado:", response.data);
        // Redirección al home
        this.$router.push("/proveedores");
        Swal.fire({
          icon: "success",
          title: "Registro exitoso",
        });
      } catch (error) {
        console.error("Error al agregar proveedor:", error);
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
