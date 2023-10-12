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
                <v-btn class="btn" icon color="#CF010B"><v-icon>mdi-trash-can-outline </v-icon></v-btn>
                <v-btn class="btnEdit" icon color="#5995fd"><v-icon>mdi-account-edit-outline </v-icon></v-btn>
            </v-col>
        </v-row>
    </v-card>



</template>

<script>
export default {
    props: {
        supplier: Object
    }
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