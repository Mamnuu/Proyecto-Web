<template>
  <v-container class="container-p">
    <h3 class="titulo">Vendedores</h3>
    <br />
    <v-row>
      <v-col v-for="seller in sellers" :key="seller.id" cols="12" md="4" sm="6">
        <v-card
          class="sellersCard"
          max-width="300"
          max-height="200"
          elevation="3"
        >
          <v-row>
            <v-col>
              <v-img :src="seller.img" class="imgv"></v-img>
            </v-col>
            <v-col>
              <v-row>
                <br />
                <v-card-title class="titulov">
                  {{ seller.nombre }}
                </v-card-title>
              </v-row>
              <v-row>
                <v-btn class="btnCardv" variant="text"> Ver vendedor </v-btn>
              </v-row>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
<script setup>
import axios from "axios";
const sellers = ref([]);

const getSellers = async () => {
  const url = "http://localhost:3001/sellers";
  const response = await axios.get(url);
  sellers.value = response.data;
};
getSellers();
</script>

<style>
.titulov {
  font-family: sans-serif;
  font-weight: bold;
  text-align: center;
  font-size: 20px !important;
  margin-left: 20px !important;
  margin-top: 10px;
}

.btnCardv {
  color: #5995fd !important;
  margin-right: 25px !important;
  margin-top: 10px;
  margin-bottom: 20px;
}

.sellersCard {
  background-color: whitesmoke !important;
  color: black;
  border-radius: 3px !important;
  width: 300px !important;
  height: 100px !important;
  align-items: center !important;
}
</style>
