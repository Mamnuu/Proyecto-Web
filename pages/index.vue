<template>
  <div class="container">
    <div class="forms-container">
      <div class="signin-signup">
        <form action="#" class="sign-in-form" @submit.prevent="handleSubmit">
          <h2 class="title">Bienvenido</h2>
          <h2 class="title">Ingresa aquí</h2>
          <div class="input-field">
            <i class="fas fa-user"></i>
            <input v-model="email" placeholder="Correo electrónico" />
          </div>
          <div class="input-field">
            <i class="fas fa-lock"></i>
            <input
              v-model="password"
              type="password"
              placeholder="Contraseña"
            />
          </div>
          <input type="submit" value="Ingresar" class="btn solid" />
        </form>
      </div>
    </div>

    <div class="panels-container">
      <div class="panel left-panel">
        <div class="content">
          <h3 class="tit">EasyMA</h3>
          <br />
          <p class="p">
            Optimiza tu tiempo y maximiza tus resultados con el CRM web EasyMA.
            Conecta tus datos, equipos y clientes en una plataforma de CRM que
            crece junto a tu negocio.
          </p>
        </div>
        <img src="/images/log.svg" class="image" alt="" />
      </div>
    </div>
  </div>
</template>

<style src="./styles.index.css"></style>
<script setup>
import axios from "axios";
import Swal from "sweetalert2";
import * as config from "../config/default.json";

onBeforeMount(() => {
  signout();
});
const signout = () => {
  localStorage.removeItem("token");
};
const email = ref("");
const password = ref("");
const errorMessage = ref("");

const handleSubmit = async () => {
  // Validación del correo electrónico
  if (!email.value || !/^\S+@\S+\.\S+$/.test(email.value)) {
    errorMessage.value = "Por favor, ingresa un correo electrónico válido.";
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: errorMessage.value,
    });
    return;
  }

  // Validación de la contraseña
  if (!password.value) {
    errorMessage.value = "Por favor, ingresa tu contraseña.";
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: errorMessage.value,
    });
    return;
  }

  errorMessage.value = "";
  await login();
};

const login = async () => {
  try {
    const url = `${config.api_host}/login`;
    const { data } = await axios.post(url, {
      email: email.value,
      password: password.value,
    });

    if (data?.ok == true) {
      // Redireccionar al home, guardar el token
      console.log(data?.info);
      const token = data?.info?.token;
      const name = data?.info?.name;
      const email = data?.info?.email;
      localStorage.setItem("token", token);
      localStorage.setItem("name", name);
      localStorage.setItem("email", email);
      useRouter().push("/home");
      // useRoute(')
    } else {
      Swal.fire({
        title: "Error!",
        text: data?.message,
        icon: "error",
      });
    }
  } catch (error) {
    console.error(error);
    Swal.fire({
      title: "Error!",
      text: "Ha ocurrido un error al conectarse.",
      icon: "error",
    });
  }
};

definePageMeta({
  layout: "blank",
});
</script>

<style>
.tit {
  font-size: 50px !important;
}
</style>
