<template>
    <v-card class="informacion overflow-y-hidden">
        <v-row>
            <v-col class="" cols="6">
                <v-card class="cardImg">
                    <v-img aspect-ratio="1/4" class="imagenP fill-height" cover :src="producto.img">
                    </v-img>

                </v-card>
                <h3 class="precio">
                    <v-icon>mdi-currency-usd</v-icon>
                    {{ producto.precio }}
                </h3>

            </v-col>
            <v-col cols="6">
                <v-row class="info">
                    <v-col>
                        <br>
                        <h3>{{ producto.nombre }}</h3>
                        <p>
                            <br>
                            {{ producto.descripcion }}
                        </p>
                        <br>
                        <v-btn @click="productDelete(producto)" color="#CF010B">Eliminar</v-btn>
                        <v-btn color="#5995fd" class="btnE">Editar</v-btn>
                    </v-col>

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
    producto: {
        type: Object,
        required: true
    },
});

const deleteProduct = async (product) => {
    const url = `http://localhost:3001/products/${product.id}`
    const { data } = await axios.delete(url)
}

const productDelete = (product) => {
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
                deleteProduct(product);
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
.cardImg {
    margin-left: 10%;
    display: flex;
    border-radius: 50% !important;
    width: 250px !important;
    height: 250px !important;
    text-align: center;
    margin-top: 8%;

}

.precio {
    color: grey;
    margin-left: 30%;
    margin-top: 4%;
}

.btnE {
    margin-left: 10%;
}

.btnCerrar {
    margin-left: 90%;
    margin-top: 2%;
}

.informacion {
    height: 340px;
    width: 725px;
    display: flex !important;
    align-items: center !important;
    justify-content: center !important;
    flex-direction: column !important;
}

.info {
    margin-bottom: 5%;
    margin-left: -10%;
    display: flex;
    justify-content: center;
    /* Alineación horizontal al centro */
    align-items: center;
    margin-top: 5%;
}
</style>
