<template>
    <v-card class="informacionV overflow-y-hidden" >
        <v-row>
            <v-col class="col1" cols="6">
                <h3 class="nombreV">
                    {{ seller.nombre + " " +  seller.apellido }}
                </h3>
                
                <v-card class="cardimgv">
                <v-img  aspect-ratio="1/4" class="imagenV" cover
                    :src=" seller.img ">
                </v-img>
            
                </v-card>
               
            </v-col>
            <v-col cols="6">
                <v-row class="infov">
                    <v-col>
                        <ul class="ulInfoV">
                                <li><b>Cargo:</b></li>
                                <li class="li"> {{ seller.cargo }}</li> <br>
                                <li><b>Correo eléctronico: </b></li>
                                <li class="li"> {{ seller.correo }}</li> <br>
                                <li><b>Contacto: </b></li>
                                <li class="li"> {{ seller.contacto }}</li> 
                        </ul>
                    </v-col>
                </v-row>
                <v-row class="btnsV"> 
                    <v-btn  @click="sellerDelete(seller)" class="btn1v" icon color="#CF010B"><v-icon>mdi-trash-can-outline </v-icon></v-btn>
                    <v-btn @click="editSeller" class="btn2v" icon color="#5995fd"><v-icon>mdi-account-edit-outline </v-icon></v-btn>
                </v-row>
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
    seller: {
        type: Object,
        required: true
    },
});

const editSeller = () => {
    // Obtiene el id del producto
    const sellerId = props.seller.id;

    // Navega a la ruta editar_producto/{id}
    router.push({
        path: `/editar_vendedor/${sellerId}`,
    });
};


const deleteSeller= async (seller) => {
    const url = `http://localhost:3001/sellers/${seller.id}`
    const { data } = await axios.delete(url)
}
const sellerDelete = (seller) => {
    emit('closeDialog')
    let error = false
    Swal.fire({
        title: 'Estás seguro?',
        text: "No podrás revertir esta acción!",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Sí, bórralo!',
        cancelButtonText: 'Cancelar',
    }).then((result) => {
        if (result.isConfirmed) {
            try {
                deleteSeller(seller);
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
    .informacionV{
        width: 450px !important; /* Establece el ancho deseado */
        height: 290px !important;
        display: flex !important;
        align-items: center !important;
        justify-content: center !important;
        flex-direction: column !important;
    }
    .nombre{
        font-family: sans-serif;
        font-weight: bold;
        text-align: center;
        margin-top: 6%;
    }

    .btnEdit{
        margin-left: 30%;
    }


    .col1{
        margin-top: 5%;
        justify-content: center;
    }

    .cardimgv {
        margin-left: 20%;
        display:flex;
        border-radius: 50% !important;
        width: 120px !important;
        height: 120px !important;
        text-align: center;
        margin-top: 8%;
        
    }

    .ulInfoV{
        list-style-type: none;
       
    }

    .btn1v{
        margin-left: 5%;
    }

    .btn2v{
        margin-right: 20%;
    }

    .btnsV{
        margin-bottom: 10%;
        display: flex;
        justify-content: space-between;
    }

   

    .infov{
        margin-top: 10%;
        margin-right: 3%;
        display: flex ;
        flex-direction: column;
        text-align: left;
        line-height: 1.5;
    }
    
    .nombreV{
        text-align: center;
        margin-top: 10%;
    }
    
</style>