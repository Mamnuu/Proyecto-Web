<template>
    <v-card class="informacionPro overflow-y-hidden">
        <v-row>
            <v-col class="col1" cols="6">
                <h3 class="nombre">
                    {{ provider.nombre }}
                    <br>
                </h3>
                <v-card class="cardimgpr">
                    <v-img aspect-ratio="1/4" class="imagenPro" cover :src="provider.img">
                    </v-img>

                </v-card>

            </v-col>
            <v-col class="col2" cols="6">
                <v-row class="infoPro">
                    <v-col>
                        <ul class="ulInfo">
                            <li><b>Producto: </b> </li>
                            <li class="li">{{ provider.producto }}</li> <br>
                            <li><b>Correo eléctronico:</b> </li>
                            <li class="li">{{ provider.correo }}</li> <br>
                            <li><b>Contacto: </b> </li>
                            <li class="li">{{ provider.contacto }}</li>
                        </ul>
                    </v-col>
                </v-row>
                <v-row class="btnsP">
                    <v-btn @click="providerDeleteAlert(provider)" class="btn1" icon
                        color="#CF010B"><v-icon>mdi-trash-can-outline
                        </v-icon></v-btn>
                    <v-btn @click="editProvider" class="btn2" icon color="#5995fd"><v-icon>mdi-account-edit-outline
                        </v-icon></v-btn>
                </v-row>


            </v-col>
        </v-row>
    </v-card>
</template>

<script setup>

import axios from "axios";
import Swal from "sweetalert2";
import * as config from "../config/default.json";
import { getHeaders } from "~/src/auth/jwt.js";

const router = useRouter();
const emit = defineEmits(['closeDialog']);

const props = defineProps({
    provider: {
        type: Object,
        required: true
    },
});

const editProvider = () => {
    // Obtiene el id del producto
    const providerId = props.provider.id;

    // Navega a la ruta editar_producto/{id}
    router.push({
        path: `/editar_proveedor/${providerId}`,
    });
};

const deleteProvider = async (supplier) => {
    const url = `${config.api_host}/providers/${supplier._id}`;
    const token = localStorage.getItem("token")
    const headers = getHeaders(token);
    const { data } = await axios.delete(url,{ headers })
}

const providerDeleteAlert = (provider) => {
    emit('closeDialog')
    let error = false
    Swal.fire({
        title: '¿Estás seguro?',
        text: "¡No podrás revertir esta acción!",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#d33',
        cancelButtonColor: '#3085d6',
        confirmButtonText: '¡Sí, bórralo!',
        cancelButtonText: 'Cancelar',
    }).then((result) => {
        if (result.isConfirmed) {
            try {
                deleteProvider(provider);
            } catch (error) {
                error = true
                console.log(error);
            }
            if (error) {
                Swal.fire({
                    icon: 'error',
                    title: 'Oops...',
                    text: 'No pudo ser posible el borrado!',
                })
            }
            else {
                Swal.fire(
                    'Borrado!',
                    'Producto borrado con éxito.',
                    'success'
                )
            }
        }
    })
}

</script>

<style>
.informacionPro {
    width: 450px !important;
    /* Establece el ancho deseado */
    height: 310px !important;
    display: flex !important;
    align-items: center !important;
    justify-content: center !important;
    flex-direction: column !important;
}

.cardimgpr {
    margin-left: 23%;
    display: flex;
    border-radius: 50% !important;
    width: 120px !important;
    height: 120px !important;
    text-align: center;
    margin-top: 10%;

}

.col1 {
    width: 330px;
    height: 260px;
}

.col2 {
    width: 360px !important;
    height: 300px !important;
}

.ulInfo {
    list-style-type: none;

}

li {
    font-family: 'Open Sans', sans-serif !important;
    text-align: justify !important;

}

.btnsP {
    margin-top: 5% !important;
    display: flex;
    justify-content: space-between;
}

.btn1 {
    margin-left: 5%;
}

.btn2 {
    margin-right: 20%;
}

.infoPro {
    margin-top: 10%;
    margin-right: 3%;
    display: flex;
    flex-direction: column;
    text-align: left;
    line-height: 1.5;

}

.nombre {
    text-align: center;
    margin-top: 10%;
}
</style>