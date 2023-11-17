<template>
  <div class="register-container">
    <div class="form-container">
      <v-card class="mx-auto pa-6 pb-2" elevation="20" max-width="448" rounded="lg">
        <v-card-title style="text-align: center">Productos</v-card-title>
        <v-card-subtitle style="text-align: center">Formulario para la creación de productos.</v-card-subtitle>
        <br />
        <div class="text-subtitle-1 text-medium-emphasis">Nombre</div>

        <v-text-field v-model="nombre" density="compact" placeholder="Nombre" prepend-inner-icon="mdi-account-outline"
          variant="underlined" :rules="Rules"></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">Precio</div>

        <v-text-field v-model.number="precio" density="compact" placeholder="Precio" prepend-inner-icon="mdi-cash"
          variant="underlined" :rules="[precioRules, Rules].flat()"></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">Descripción</div>

        <v-textarea name="input-7-4" v-model="descripcion" density="compact"
          prepend-inner-icon="text-box-outline"></v-textarea>

        <div class="text-subtitle-1 text-medium-emphasis">Subir imágen</div>

        <v-file-input v-model="image" label="Subir"></v-file-input>

        <v-btn block class="mb-8" color="#5995fd" size="large" variant="outlined" @click="register">
          Registrar
        </v-btn>
      </v-card>
    </div>
  </div>
</template>

<script>
definePageMeta({
  layout: "default",
});
import axios from "axios";
import Swal from "sweetalert2";
import config from "~/config/default.json";
import { getHeaders } from "~/src/auth/jwt.js";
let nextProductId = 8; // Contador para el ID secuencial

const errorMessage = ref("");

export default {
  data: () => {
    return {
      Rules: [(v) => !!v || "El campo es obligatorio"],
      precioRules: [
        (v) => /^[-+]?[0-9]*\.?[0-9]*$/.test(v) || "Solo se permiten números",
      ],
      nombre: "",
      precio: "",
      descripcion: "",
      image: null,
      products: [],
    };
  },
  methods: {
    async register() {
      // Validación campos vacíos

      try {
        if (this.nombre == "" || this.precio == "" || this.descripcion == "") {
          errorMessage.value = "Por favor, ingresa todos los campos.";
          Swal.fire({
            icon: "error",
            title: "Oops...",
            text: errorMessage.value,
          });
          return;
        }

        // Validación de solo números
        if (!/[0-9-]+/.test(this.precio)) {
          errorMessage.value = "Solo se permiten números";
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
          console.error("El producto ya está registrado");
        } else {
          // Generación de ID secuencial única para el producto
          const productId = nextProductId;

          // Incrementa el contador para el siguiente usuario
          nextProductId++;

          // Creación de objeto FormData para enviar la imagen


          // Registra al usuario con el ID generado
          const product = {
            id: productId,
            name: this.nombre,
            price: this.precio,
            description: this.descripcion,
          };


          const url = `${config.api_host}/products`;
          const token = localStorage.getItem("token");
          const headers = getHeaders(token);

          const { data: dataProduct } = await axios.post(url, product, { headers });
          const { info: infoProduct } = dataProduct;

          const formData = new FormData();
          formData.append("img", this.image[0])
          console.log("formData", formData);
          const { data } = await axios.post(`${url}/${infoProduct._id}/image_profile`, formData, { headers })

          // Redirección al home
          this.$router.push("/home");
          Swal.fire({
            icon: "success",
            title: "Registro exitoso",
          });

          console.log("", formData);
        }
      } catch (error) {
        console.error("Error al agregar producto", error);
        Swal.fire({
          icon: "error",
          title: "Error en el registro",
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
