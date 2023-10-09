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
                        <input v-model="password" type="password" placeholder="Contraseña" />
                    </div>
                    <input type="submit" value="Ingresar" class="btn solid" />
                </form>
            </div>
        </div>

        <div class="panels-container">
            <div class="panel left-panel">
                <div class="content">
                    <h3 class="tit">EasyMA</h3>
                    <br>
                    <p class="p">
                        Optimiza tu tiempo y maximiza tus resultados con el CRM web EasyMA.
                         Conecta tus datos, equipos y clientes en una plataforma de CRM 
                         que crece junto a tu negocio.
                    </p>
                </div>
                <img src="/images/log.svg" class="image" alt="" />
            </div>
        </div>
    </div>
</template>

<style  src="./styles.index.css"></style>
<script setup>
import axios from "axios";
import Swal from "sweetalert2";

const router = useRouter();
const email = ref('');
const password = ref('');
const errorMessage = ref('');
const users = ref([])

const handleSubmit = async () => {
    // Validación del correo electrónico
    if (!email.value || !/^\S+@\S+\.\S+$/.test(email.value)) {
        errorMessage.value = 'Por favor, ingresa un correo electrónico válido.';
        Swal.fire({
            icon: 'error',
            title: 'Oops...',
            text: errorMessage.value,
        })
        return;
    }

    // Validación de la contraseña
    if (!password.value) {
        errorMessage.value = 'Por favor, ingresa tu contraseña.';
        Swal.fire({
            icon: 'error',
            title: 'Oops...',
            text: errorMessage.value,
        })
        return;
    }

    errorMessage.value = '';
    await login();
};

const getUsers = async () => {
    const url = "http://localhost:3001/users"
    const response = await axios.get(url)
    users.value = response.data
}

const login = async () => {
    await getUsers()
    console.log(users.value);
    const foundUser = users.value.find(
        user =>
            user.email === email.value && user.password === password.value
    );
    if (foundUser) {
        console.log('Inicio de sesión exitoso para el usuario:', foundUser);
        router.push('/home');
    } else {
        Swal.fire({
            icon: 'error',
            title: 'Oh no...',
            text: 'Credenciales incorrectas, inténtalo de nuevo!',

        })
    }
}

definePageMeta({
    layout:"blank",
})
</script>

<style>
.tit{
    font-size: 50px !important;
}
</style>



