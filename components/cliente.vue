<template>
    <v-card class="informacionC overflow-y-hidden">
        <v-row>
            <v-col class="col1" cols="6">
                <h3 class="nombreC">
                    {{ customer.nombre + " " + customer.apellido }}
                </h3>
                <v-card class="cardimgc">
                    <v-img aspect-ratio="1/4" class="imagenC" cover :src="customer.img">
                    </v-img>

                </v-card>

            </v-col>
            <v-col cols="6">
                <v-row class="infoC">
                    <v-col>
                        <ul class="ulInfoC">
                            <li><b> Dirección: </b></li>
                            <li class="li">{{ customer.direccion }}</li> <br>
                            <li><b>Correo eléctronico:</b></li>
                            <li class="li"> {{ customer.correo }}</li>
                        </ul>
                    </v-col>
                </v-row>
                <v-row class="btnsC"> 
                    <v-btn @click="productCustomer(customer)" class="btn1c" icon color="#CF010B"><v-icon>mdi-trash-can-outline </v-icon></v-btn>
                    <v-btn  @click="editCustomer" class="btn2c" icon color="#5995fd"><v-icon>mdi-account-edit-outline </v-icon></v-btn>
                </v-row>

            </v-col>
        </v-row>
    </v-card>
</template>

<script setup>
import axios from "axios";
import Swal from "sweetalert2";
const router = useRouter();
const emit = defineEmits(['closeDialog'])
const props = defineProps({
    customer: {
        type: Object,
        required: true
    },
});

const editCustomer = () => {
    // Obtiene el id del producto
    const customerId = props.customer.id;

    // Navega a la ruta editar_producto/{id}
    router.push({
        path: `/editar_cliente/${customerId}`,
    });
};


const deleteCustomer = async (customer) => {
    const url = `http://localhost:3001/customers/${customer.id}`
    const { data } = await axios.delete(url)
}

const productCustomer = (customer) => {
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
                deleteCustomer(customer);
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
.cardimgc {
    margin-left: 20%;
    display: flex;
    border-radius: 50% !important;
    width: 120px !important;
    height: 120px !important;
    text-align: center;
    margin-top: 5%;

}

.informacionC {
    width: 420px !important;
    /* Establece el ancho deseado */
    height: 250px !important;
    display: flex !important;
    align-items: center !important;
    justify-content: center !important;
    flex-direction: column !important;
    margin-top: 5%;
}


.infoC {
    margin-top: 10%;
    margin-right: 3%;

}

.ulInfoC{
    list-style-type: none;
}

.btn1c{
    margin-left: 5%;
}

.btn2c{
    margin-right: 20%;
}

.btnsC{
    margin-top: 5% !important;
    display: flex;
    justify-content: space-between;
}

.nombreC {
    text-align: center;
    margin-top: 4%;

}
</style>