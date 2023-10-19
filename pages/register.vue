<template>
  <div class="register-container">
    <div class="form-container">
      <v-card class="mx-auto pa-6 pb-2" elevation="20" max-width="448" rounded="lg">
        <v-card-title style="text-align: center">Registrar</v-card-title>
        <v-card-subtitle style="text-align: center">Formulario para registro de vendedores y admins.</v-card-subtitle>
        <br>
        <div class="text-subtitle-1 text-medium-emphasis">Nombres</div>

        <v-text-field v-model="nombre" density="compact" placeholder="Nombres" prepend-inner-icon="mdi-account-outline"
          variant="underlined" :rules="Rules"></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">Apellidos</div>

        <v-text-field v-model="apellido" density="compact" placeholder="Apellidos"
          prepend-inner-icon="mdi-account-outline" variant="underlined" :rules="Rules"></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">Cargo</div>

        <v-select v-model="cargo" density="compact" placeholder="Cargo" prepend-inner-icon="mdi-account-hard-hat-outline"
          :items="items" variant="underlined"></v-select>

        <div class="text-subtitle-1 text-medium-emphasis">
          Correo electrónico
        </div>

        <v-text-field v-model="correo" density="compact" placeholder="Correo electrónico"
          prepend-inner-icon="mdi-email-outline" variant="underlined" :rules="Rules"></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">
          Número de contacto
        </div>

        <v-text-field v-model="contacto" density="compact" placeholder="Número de contacto"
          prepend-inner-icon="mdi-card-account-phone-outline" variant="underlined"
          :rules="[Rules, contactoRules].flat()"></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">Contraseña</div>

        <v-text-field v-model="password" :append-inner-icon="visible ? 'mdi-eye-off' : 'mdi-eye'"
          :type="visible ? 'text' : 'password'" density="compact" placeholder="Ingresar la contraseña"
          prepend-inner-icon="mdi-lock-outline" variant="underlined" @click:append-inner="visible = !visible"
          :rules="Rules"></v-text-field>

        <div class="text-subtitle-1 text-medium-emphasis">
          Confirmar Contraseña
        </div>

        <v-text-field v-model="confirmPassword" :append-inner-icon="confirmVisible ? 'mdi-eye-off' : 'mdi-eye'"
          :type="confirmVisible ? 'text' : 'password'" density="compact" placeholder="Confirma la contraseña"
          prepend-inner-icon="mdi-lock-outline" variant="underlined"
          @click:append-inner="confirmVisible = !confirmVisible" :rules="Rules"></v-text-field>

        <v-btn block class="mb-8" color="#5995fd" size="large" variant="outlined" @click="register">
          Registrar
        </v-btn>
      </v-card>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Swal from "sweetalert2";
let nextUserId = 9; // Contador para el ID secuencial
const img = "/images/perfil-del-usuario.png";

const errorMessage = ref("");

export default {
  data: () => {
    return {
      Rules: [(v) => !!v || "El campo es obligatorio"],
      contactoRules: [v => (/^[0-9]+|[()\.]+/.test(v)) || 'Solo se permiten números'],
      nombre: "",
      apellido: "",
      cargo: "Seleccionar",
      items: ["Vendedor", "Administrador"],
      correo: "",
      contacto: "",
      password: "",
      confirmPassword: "",
      visible: false,
      confirmVisible: false,
    };
  },
  methods: {
    async getUsers() {
      try {
        const response = await axios.get("http://localhost:3001/sellers");
        this.users = response.data;
      } catch (error) {
        console.error("Error al obtener usuarios:", error);
      }
    },
    async register() {
      await this.getUsers();

      // Validación campos vacíos
      if (
        this.nombre == "" ||
        this.apellido == "" ||
        this.cargo == "Seleccionar" ||
        this.correo == "" ||
        this.contacto == "" ||
        this.password == "" ||
        this.confirmPassword == ""
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

      // Validación contraseñas coinciden
      if (!this.password || !this.confirmPassword) {
        errorMessage.value = "Por favor, ingresa tu contraseña.";
        Swal.fire({
          icon: "error",
          title: "Oops...",
          text: errorMessage.value,
        });
        return;
      }

      // Verificar si existe el correo
      const emailExists = this.users.some(
        (user) => user.correo === this.correo
      );

      if (emailExists) {
        console.error("El correo electrónico ya está registrado.");
      } else if (this.password !== this.confirmPassword) {
        Swal.fire({
          title: "Error",
          text: "Las contraseñas no coinciden",
          icon: "error",
        });
      } else {
        // Generación de ID secuencial única para el usuario
        const userId = nextUserId;

        // Incrementa el contador para el siguiente usuario
        nextUserId++;

        // Registra al usuario con el ID generado
        const newUser = {
          id: this.userId,
          nombre: this.nombre,
          apellido: this.apellido,
          cargo: this.cargo,
          correo: this.correo,
          contacto: this.contacto,
          password: this.password,
          img: img,
        };

        // Añade el usuario al servidor
        await this.addUser(newUser);

        console.log("", newUser);
      }
    },
    async addUser(user) {
      try {
        const response = await axios.post(
          "http://localhost:3001/sellers",
          user
        );
        console.log("Usuario agregado:", response.data);
        // Redirección al home
        this.$router.push("/vendedores");
        Swal.fire({
          icon: "success",
          title: "Registro exitoso",
        });
      } catch (error) {
        console.error("Error al agregar usuario:", error);
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
