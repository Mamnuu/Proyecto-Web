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
              <v-card class="cardimg" >
                <v-img cover :src="seller.img"  class="imgv"></v-img>
              </v-card>
            </v-col>
            <v-col>
              <v-row>
                <br />
                <v-card-title class="titulov">
                  {{ seller.nombre }} 
                </v-card-title>
              </v-row>
              <v-row>
                <v-card-actions>
                  <v-btn class="btnCardv" variant="text"  block @click="openDialog(seller);"> Ver vendedor </v-btn>
                </v-card-actions>
              </v-row>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
      <v-card v-if="dialog"  class="infor" elevation="7">
        <v-dialog v-model="dialog" width="auto" style="background: none !important; border-radius: 80px !important;">
          <DescripcionV :seller="currentSeller"  @closeDialog="closeDialog"></DescripcionV>
        </v-dialog>
      </v-card>
      <v-btn fab dark large color="#5995fd" class="btn-flotante" to="/register">
        <v-icon size="50">mdi-plus</v-icon>
      </v-btn>
    </v-row>
  </v-container>
</template>
<script setup>
import axios from "axios";
import DescripcionV from '~/components/vendedor.vue';
const sellers = ref([]);
const currentSeller = ref(null);
const dialog = ref(false);

const closeDialog = () => {
  dialog.value = false;
}

const getSellers = async () => {
  const url = "http://localhost:3001/sellers";
  const response = await axios.get(url);
  sellers.value = response.data;
};

onBeforeMount(() => {
  getSellers();
})

const openDialog = (seller) => {
  currentSeller.value = seller;
  dialog.value = true;
}
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
.imgv {
  object-fit: cover !important;
  border-radius: 20% !important;

}
.cardimg {
  display:flex;
  margin-left: 10px !important;
  border-radius: 50% !important;
  width: 100px !important;
  height: 100px !important;
}
</style>
