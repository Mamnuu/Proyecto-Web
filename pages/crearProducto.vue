<template>
  <div class="register-container">
    <div class="form-container">
      <v-card
        class="mx-auto pa-6 pb-2"
        elevation="20"
        max-width="448"
        rounded="lg"
      >
        <v-card-title style="text-align: center">Productos</v-card-title>
        <v-card-subtitle style="text-align: center"
          >Formulario para la creación de productos.</v-card-subtitle
        >
        <br />
        <div class="text-subtitle-1 text-medium-emphasis">Nombre</div>

        <v-text-field
          v-model="nombre"
          density="compact"
          placeholder="Nombre"
          prepend-inner-icon="mdi-account-outline"
          variant="underlined"
          :rules="Rules"
        ></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">Precio</div>

        <v-text-field
          v-model="precio"
          density="compact"
          placeholder="Precio"
          prepend-inner-icon="mdi-cash"
          variant="underlined"
          :rules="Rules"
        ></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">Descripción</div>

        <v-textarea
          name="input-7-4"
          v-model="descripcion"
          density="compact"
          prepend-inner-icon="text-box-outline"
          :rules="Rules"
        ></v-textarea>

        <v-btn
          block
          class="mb-8"
          color="#5995fd"
          size="large"
          variant="outlined"
          @click="register"
        >
          Registrar
        </v-btn>
      </v-card>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Swal from "sweetalert2";
let nextProductId = 6; // Contador para el ID secuencial
const img = "/images/imgChorizos.jpg";

const errorMessage = ref("");

export default {
  data: () => {
    return {
      Rules: [(v) => !!v || "El campo es obligatorio"],
      nombre: "",
      precio: "",
      descripcion: "",
    };
  },
  methods: {
    async getProducts() {
      try {
        const response = await axios.get("http://localhost:3001/products");
        this.products = response.data;
      } catch (error) {
        console.error("Error al obtenter los productos", error);
      }
    },
    async register() {
      await this.getProducts();

      // Validación campos vacíos
      if (this.nombre == "" || this.precio == "" || this.descripcion == "") {
        errorMessage.value = "Por favor, ingresa todos los campos.";
        Swal.fire({
          icon: "error",
          title: "Oops...",
          text: errorMessage.value,
        });
        return;
      }

      // Verificar si existe el producto
      const productExists = this.products.some(
        (product) => product.nombre === this.nombre
      );

      if (productExists) {
        console.error("El producto ya está registrado.");
      } else {
        // Generación de ID secuencial única para el producto
        const productId = nextProductId;

        // Incrementa el contador para el siguiente usuario
        nextProductId++;

        // Registra al usuario con el ID generado
        const newProduct = {
          id: this.productId,
          nombre: this.nombre,
          precio: this.precio,
          descripcion: this.descripcion,
          img: img,
        };

        // Añade el usuario al servidor
        await this.addProduct(newProduct);

        console.log("", newProduct);
      }
    },
    async addProduct(product) {
      try {
        const response = await axios.post(
          "http://localhost:3001/products",
          product
        );
        console.log("Producto agregado:", response.data);
        // Redirección al home
        this.$router.push("/home");
        Swal.fire({
          icon: "success",
          title: "Registro exitoso",
        });
      } catch (error) {
        console.error("Error al agregar producto:", error);
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
