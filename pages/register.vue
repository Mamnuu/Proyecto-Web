<template>
    <div class="container">
        <div class="form-container">
            <v-card class="mx-auto pa-12 pb-8" elevation="20" max-width="448" rounded="lg">
                <div class="text-subtitle-1 text-medium-emphasis">Nombres</div>

                <v-text-field v-model="fullName" density="compact" placeholder="Nombres"
                    prepend-inner-icon="mdi-account-outline" variant="underlined" :rules="Rules"></v-text-field>

                <div class="text-subtitle-1 text-medium-emphasis">Apellidos</div>

                <v-text-field v-model="lastname" density="compact" placeholder="Apellidos"
                    prepend-inner-icon="mdi-account-outline" variant="underlined" :rules="Rules"></v-text-field>

                <div class="text-subtitle-1 text-medium-emphasis">Cargo</div>

                <v-select v-model="charge" density="compact" placeholder="Cargo"
                    prepend-inner-icon="mdi-account-hard-hat-outline" :items="items" variant="underlined" :rules="Rules"></v-select>

                <div class="text-subtitle-1 text-medium-emphasis">Correo electrónico</div>

                <v-text-field v-model="email" density="compact" placeholder="Correo electrónico"
                    prepend-inner-icon="mdi-email-outline" variant="underlined" :rules="Rules"></v-text-field>

                <div class="text-subtitle-1 text-medium-emphasis">Contraseña</div>

                <v-text-field v-model="password" :append-inner-icon="visible ? 'mdi-eye-off' : 'mdi-eye'"
                    :type="visible ? 'text' : 'password'" density="compact" placeholder="Ingresar la contraseña"
                    prepend-inner-icon="mdi-lock-outline" variant="underlined" @click:append-inner="visible = !visible"
                    :rules="Rules"></v-text-field>

                <div class="text-subtitle-1 text-medium-emphasis">Confirmar Contraseña</div>

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
import axios from 'axios';
import Swal from "sweetalert2";
let nextUserId = 2;  // Contador para el ID secuencial


const errorMessage = ref('');



export default {
    data: () => {
        return {
            Rules: [
                vn => !!vn || 'El campo es obligatorio',
            ],
            fullName: '',
            lastname: '',
            charge: 'Seleccionar',
            items: [
                'Vendedor',
                'Administrador',
            ],
            email: '',
            password: '',
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

            console.log(this.fullName);
            
            // Validación campos vacíos
            if(this.fullName == '' || this.lastname == '' || this.charge == 'Seleccionar' || this.email == '' || this.password == '' || this.confirmPassword == ''){
                errorMessage.value = 'Por favor, ingresa todos los campos.';
                Swal.fire({
                    icon: 'error',
                    title: 'Oops...',
                    text: errorMessage.value,
                })
                return;
            }

            // Validación correo válido
            if (!this.email || !/^\S+@\S+\.\S+$/.test(this.email)) {
                console.log('Por favor, ingresa un correo electrónico válido.');
                errorMessage.value = 'Por favor, ingresa un correo electrónico válido.';
                Swal.fire({
                    icon: 'error',
                    title: 'Oops...',
                    text: errorMessage.value,
                })
                return;
            }

            // Validación contraseñas coinciden
            if (!this.password || !this.confirmPassword) {
                errorMessage.value = 'Por favor, ingresa tu contraseña.';
                Swal.fire({
                    icon: 'error',
                    title: 'Oops...',
                    text: errorMessage.value,
                })
                return;
            }

            // Verificar si existe el correo
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
                // Generación de ID secuencial única para el usuario
                const userId = nextUserId;

                // Incrementa el contador para el siguiente usuario
                nextUserId++;

                // Registra al usuario con el ID generado
                const newUser = {
                    id: userId,
                    fullName: this.fullName,
                    lastname: this.lastname,
                    charge: this.charge,
                    email: this.email,
                    password: this.password
                };

                
            }
        },


        
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
    padding-top: 1%;
    padding-bottom: 5%;

}

.form-container v-card {
    width: 100%;
    /* Ajusta el ancho de la tarjeta al 100% del contenedor */

}
</style>