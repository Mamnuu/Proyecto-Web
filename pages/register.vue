<template>
    <div class="container">
        <div class="form-container">
            <v-card class="mx-auto pa-12 pb-8" elevation="20" max-width="448" rounded="lg">
                <div class="text-subtitle-1 text-medium-emphasis">Nombres</div>

                <v-text-field v-model="fullName" density="compact" placeholder="Nombres"
                    prepend-inner-icon="mdi-account-outline" variant="underlined" :rules="fullnameRules"></v-text-field>

                <div class="text-subtitle-1 text-medium-emphasis">Apellidos</div>

                <v-text-field v-model="lastname" density="compact" placeholder="Apellidos"
                    prepend-inner-icon="mdi-account-outline" variant="underlined" :rules="lastnameRules"></v-text-field>

                <div class="text-subtitle-1 text-medium-emphasis">Cargo</div>

                <v-text-field v-model="charge" density="compact" placeholder="Cargo"
                    prepend-inner-icon="mdi-account-hard-hat-outline" variant="underlined" :rules="chargeRules"></v-text-field>

                <div class="text-subtitle-1 text-medium-emphasis">Correo electrónico</div>

                <v-text-field v-model="email" density="compact" placeholder="Correo electrónico"
                    prepend-inner-icon="mdi-email-outline" variant="underlined" :rules="emailRules"></v-text-field>

                <div class="text-subtitle-1 text-medium-emphasis">Contraseña</div>

                <v-text-field v-model="password" :append-inner-icon="visible ? 'mdi-eye-off' : 'mdi-eye'"
                    :type="visible ? 'text' : 'password'" density="compact" placeholder="Ingresar la contraseña"
                    prepend-inner-icon="mdi-lock-outline" variant="underlined"
                    @click:append-inner="visible = !visible" :rules="passwordRules"></v-text-field>

                <div class="text-subtitle-1 text-medium-emphasis">Confirmar Contraseña</div>

                <v-text-field v-model="confirmPassword" :append-inner-icon="confirmVisible ? 'mdi-eye-off' : 'mdi-eye'"
                    :type="confirmVisible ? 'text' : 'password'" density="compact" placeholder="Confirma la contraseña"
                    prepend-inner-icon="mdi-lock-outline" variant="underlined"
                    @click:append-inner="confirmVisible = !confirmVisible"></v-text-field>

                <v-btn block class="mb-8" color="#5995fd" size="large" variant="outlined" @click="register">
                    Registrar vendedor
                </v-btn>


            </v-card>
        </div>
    </div>
</template>


<script>
import axios from 'axios';
import Swal from "sweetalert2";
let nextUserId = 2;  // Contador para el ID secuencial


const fullName = ref('');
const lastname = ref('');
const charge = ref('');
const email = ref('');
const password = ref('');
const confirmPassword = ref('');
const errorMessage = ref('');


export default {
    data() {
        return {
            fullName: '',
            fullnameRules: [
                vn => !!vn || 'El nombre es requerido',
            ],
            lastname: '',
            lastnameRules: [
                vl => !!vl || 'El apellido es requerido',
            ],
            charge: '',
            chargeRules: [
                vc => !!vc || 'El cargo es requerido',
            ],
            email: '',
            emailRules: [
                ve => !!ve || 'El correo es requerido',
            ], 
            password: '',
            passwordRules: [
                vp => !!vp || 'La contraseña es requerida',
            ],
            confirmPassword: '',
            visible: false,
            confirmVisible: false,
            users: []
        };
    },
    methods: {
        async getUsers() {
            try {
                const response = await axios.get('http://localhost:3001/users');
                this.users = response.data;
            } catch (error) {
                console.error('Error al obtener usuarios:', error);
            }
        },
        async register() {
            await this.getUsers();


            if (!email.value || !/^\S+@\S+\.\S+$/.test(email.value)) {
        errorMessage.value = 'Por favor, ingresa un correo electrónico válido.';
        Swal.fire({
            icon: 'error',
            title: 'Oops...',
            text: errorMessage.value,
        })
        return;
    }
            // Check if the email already exists
            const emailExists = this.users.some(user => user.email === this.email);

            if (emailExists) {
                console.error('El correo electrónico ya está registrado.');
            } else if (this.password !== this.confirmPassword) {
                Swal.fire({
                    title: "Error",
                    text: "Las contraseñas no coinciden",
                    icon: 'error',
                })
            } else {
                // Generate a unique sequential ID for the new user
                const userId = nextUserId;

                // Increment the counter for the next user
                nextUserId++;

                // Register the new user with the generated sequential ID
                const newUser = {
                    id: userId,
                    fullName: this.fullName,
                    lastname: this.lastname,
                    charge: this.charge,
                    email: this.email,
                    password: this.password
                };

                // Add the user to the server
                await this.addUser(newUser);

                Swal.fire(
                    {
                        icon: 'success',
                        title: 'Registro exitoso:'
                    }
                )

                console.log('', newUser);
                // Optionally, you can redirect to a different page after successful registration
                this.$router.push('./');
            }
        },


        async addUser(user) {
            try {
                const response = await axios.post('http://localhost:3001/users', user);
                console.log('Usuario agregado:', response.data);
            } catch (error) {
                console.error('Error al agregar usuario:', error);
            }
        }
    }
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
    height: 100vh;
    padding-top: 2%;
    padding-bottom: 5%;

}

.form-container v-card {
    width: 100%;
    /* Ajusta el ancho de la tarjeta al 100% del contenedor */

}

</style>