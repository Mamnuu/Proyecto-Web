
<template>
    <v-card class="informacion overflow-y-hidden" >
        <v-row>
            <v-col class="" cols="6">
                <v-card class="cardImg">
                <v-img  aspect-ratio="1/4" class="imagenP fill-height" cover
                    :src=" producto.img ">
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
                        <v-btn color="#CF010B">Eliminar</v-btn>
                        <v-btn color="#5995fd" class="btnE">Editar</v-btn>
                    </v-col>
                
                </v-row>  
            </v-col>
        </v-row>
    </v-card>
</template>

<script >
export default {
    props: {
        producto: Object
    }
}

const dialog = ref(false);
const currentProduct = ref(null);

const openDialog = (producto) => {
  currentProduct.value = producto;
  dialog.value = true;
}


</script>
<style>
    .cardImg {
        margin-left: 10%;
        display:flex;
        border-radius: 50% !important;
        width: 250px !important;
        height: 250px !important;
        text-align: center;
        margin-top: 8%;
        
    }

    .precio{
        color: grey;
        margin-left: 30%;
        margin-top: 4%;
    }
    
    .btnE{
        margin-left: 10%;
    }

    .btnCerrar{
        margin-left: 90%;
        margin-top: 2%;
    }

    .informacion{
        height: 340px;
        width: 725px;
        display: flex !important;
        align-items: center !important;
        justify-content: center !important;
        flex-direction: column !important;
    }

    .info{
        margin-bottom: 5%;
        margin-left: -10%;
        display: flex;
        justify-content: center; /* Alineación horizontal al centro */
        align-items: center; 
        margin-top: 5%;
    }
</style>
