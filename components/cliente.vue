<template>
     <v-card class="informacionC overflow-y-hidden" >
        <v-row>
            <v-col class="col1" cols="6">
                <h3 class="nombreC">
                    {{ customer.nombre + " " + customer.apellido}}
                </h3>
                <v-card class="cardimgc">
                <v-img  aspect-ratio="1/4" class="imagenC" cover
                    :src=" customer.img ">
                </v-img>
            
                </v-card>
               
            </v-col>
            <v-col cols="6">
                <v-row class="infoC">
                    <v-col>
                        <ul>
                                <li class="li"> Dirección: {{ customer.direccion }}</li> <br>
                                <li class="li"> Correo eléctronico: {{ customer.correo }}</li> 
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
        customer: Object
    }
    }
</script>

<style>

    .cardimgc{
        margin-left: 20%;
        display:flex;
        border-radius: 50% !important;
        width: 120px !important;
        height: 120px !important;
        text-align: center;
        margin-top: 2%;
        
    }

    .informacionC{
        width: 420px !important; /* Establece el ancho deseado */
        height: 230px !important;
        display: flex !important;
        align-items: center !important;
        justify-content: center !important;
        flex-direction: column !important;
    }

    li{
        font-family: sans-serif;
        font-weight: bold;
        text-align: center;
        
    }

    .infoC{
        margin-top: 10%;
        margin-right: 3%;
    
    }

    .nombreC{
        font-family: sans-serif;
        font-weight: bold;
        text-align: center;
       
    }

</style>