<template>
  <v-container class="container-p">
    <h3 class="titulo">Proveedores</h3>
    <br />
    <v-row>
      <v-col v-for="supplier in providers" :key="supplier.id" cols="12" md="4" sm="6">
        <v-card class="supplierCard" max-width="300" max-height="200" elevation="3">
          <v-row>
            <v-col>
              <v-card class="cardimg">
                <v-img cover :src="supplier.img" class="imgv"></v-img>
              </v-card>
            </v-col>
            <v-col>
              <v-row>
                <br />
                <v-card-title class="titulop">
                  {{ supplier.nombre }}
                </v-card-title>
              </v-row>
              <v-row>
                <v-btn class="btnCardp" variant="text" block @click="openDialog(supplier);"> Ver proveedor </v-btn>
              </v-row>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
      <v-card v-if="dialog" class="infopro" elevation="7">
        <v-dialog v-model="dialog" width="auto" style="background: none !important; border-radius: 80px !important;">
          <DescripcionPro :provider="currentSupplier" @closeDialog="closeDialog"></DescripcionPro>
          <v-btn class="btnCerrar " @click="dialog = false" icon elevation="0"><v-icon
              class="iconC">mdi-close</v-icon></v-btn>
        </v-dialog>
      </v-card>
      <v-btn fab dark large color="#5995fd" class="btn-flotante" to="/registrarProveedor">
        <v-icon size="50">mdi-plus</v-icon>
      </v-btn>
    </v-row>
  </v-container>
</template>
<script setup>
import axios from "axios";
import DescripcionPro from '~/components/proveedor.vue';
import * as config from "../config/default.json";
import { getHeaders } from "~/src/auth/jwt.js";

const providers = ref([]);
const dialog = ref(false);
const currentSupplier = ref(null);

const getProviders = async () => {
  const url = `${config.api_host}/providers`;
  const token = localStorage.getItem("token")
  const headers = getHeaders(token);
  const { data } = await axios.get(url, { headers });
  providers.value = data.info;

};

onBeforeMount(() => {
  getProviders();
})

const openDialog = (supplier) => {
  currentSupplier.value = supplier;
  dialog.value = true;
}

const closeDialog = () => {
  dialog.value = false;
  getProviders();
}

watch(dialog, (newValue) => {
  if (!newValue) {
    dialog.value = false;
    getProviders();
  }
});


</script>

<style>
.titulop {
  font-family: sans-serif !important;
  font-weight: bold !important;
  text-align: center !important;
  font-size: 20px !important;
  margin-left: 10px !important;
  margin-top: 10px !important;
}

.btnCardp {
  color: #5995fd !important;
  margin-right: 25px !important;
  margin-top: 10px;
  margin-bottom: 20px;
}

.supplierCard {
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
  display: flex;
  margin-left: 10px !important;
  border-radius: 50% !important;
  width: 100px !important;
  height: 100px !important;
}
</style>
