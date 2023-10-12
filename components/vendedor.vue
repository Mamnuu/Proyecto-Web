<template>
    <v-card class="informacionV overflow-y-hidden" >
        <v-row>
            <v-col class="col1" cols="6">
                <h4 class="nombre">
                    {{ seller.nombre + " " +  seller.apellido }}
                </h4>
                <v-card class="cardimgv">
                <v-img  aspect-ratio="1/4" class="imagenV" cover
                    :src=" seller.img ">
                </v-img>
            
                </v-card>
               
            </v-col>
            <v-col cols="6">
                <v-row class="infov">
                    <v-col>
                        <ul>
                             <li class="li"> Cargo: {{ seller.cargo }}</li> <br>
                                <br>
                                <li class="li"> Correo eléctronico: {{ seller.correo }}</li> <br>
                                <li class="li"> Contacto: {{ seller.contacto }}</li> <br>
                        </ul>
                    </v-col>
                </v-row>
                <v-btn  icon color="#CF010B"><v-icon>mdi-trash-can-outline </v-icon></v-btn>
                <v-btn class="btnEdit" icon color="#5995fd"><v-icon>mdi-account-edit-outline </v-icon></v-btn>
            </v-col>
        </v-row>
    </v-card>


</template>

<script>

export default {
    props: {
        seller: Object
    }
}

</script>

<style>
    .informacionV{
        width: 420px !important; /* Establece el ancho deseado */
        height: 300px !important;
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
        margin-left: 14%;
        display:flex;
        border-radius: 50% !important;
        width: 120px !important;
        height: 120px !important;
        text-align: center;
        margin-top: 8%;
        
    }

    li{
        font-family: sans-serif;
        font-weight: bold;
        text-align: center;
    }

    .infov{
        margin-top: 10%;
    }

</style>