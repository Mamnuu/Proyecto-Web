<template>
     <v-card class="informacionPro overflow-y-hidden" >
        <v-row>
            <v-col class="col1" cols="6">
                <h3 class="nombre">
                    {{ supplier.nombre }}
                </h3>
                <v-card class="cardimgpr">
                <v-img  aspect-ratio="1/4" class="imagenPro" cover
                    :src=" supplier.img ">
                </v-img>
            
                </v-card>
               
            </v-col>
            <v-col cols="6">
                <v-row class="infoPro">
                    <v-col>
                        <ul>
                                <li class="li"> Producto: {{ supplier.producto }}</li> <br>
                                <li class="li"> Correo eléctronico: {{ supplier.correo }}</li> 
                                <li class="li"> Contacto: {{ supplier.contacto }}</li> 
                        </ul>
                    </v-col>
                </v-row>
                <v-btn  @click="supplierDelete(supplier)" class="btn" icon color="#CF010B"><v-icon>mdi-trash-can-outline </v-icon></v-btn>
                <v-btn @click="editSupplier" class="btnEdit" icon color="#5995fd"><v-icon>mdi-account-edit-outline </v-icon></v-btn>
            </v-col>
        </v-row>
    </v-card>

</template>

<script setup>

import axios from "axios";
import Swal from "sweetalert2";
const router = useRouter();
const emit = defineEmits(['closeDialog']);

const props = defineProps({
    supplier: {
        type: Object,
        required: true
    },
});

const editSupplier = () => {
    // Obtiene el id del producto
    const supplierId = props.supplier.id;

    // Navega a la ruta editar_producto/{id}
    router.push({
        path: `/editar_proveedor/${supplierId}`,
    });
};

const deleteSupplier= async (supplier) => {
    const url = `http://localhost:3001/suppliers/${supplier.id}`
    const { data } = await axios.delete(url)
}

const supplierDelete = (supplier) => {
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
                deleteSupplier(supplier);
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

    .informacionPro{
        width: 450px !important; /* Establece el ancho deseado */
        height: 280px !important;
        display: flex !important;
        align-items: center !important;
        justify-content: center !important;
        flex-direction: column !important;
    }

    .cardimgpr{
        margin-left: 20%;
        display:flex;
        border-radius: 50% !important;
        width: 120px !important;
        height: 120px !important;
        text-align: center;
        margin-top: 4%;
        
    }

    li{
        font-family: sans-serif;
        font-weight: bold;
        text-align: center;
        
    }

    .infoPro{
        margin-top: 10%;
        margin-right: 3%;
    
    }




</style>