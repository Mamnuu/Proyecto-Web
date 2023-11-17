<template>
  <v-container class="container-p">
    <h3 class="titulo">Clientes</h3>
    <br />
    <v-row>
      <v-col
        v-for="customer in customers"
        :key="customer.id"
        cols="12"
        md="4"
        sm="6"
      >
        <v-card
          class="customerCard"
          max-width="300"
          max-height="200"
          elevation="3"
        >
          <v-row>
            <v-col>
              <v-card class="cardimg">
                <v-img cover :src="customer.img" class="imgv"></v-img>
              </v-card>
            </v-col>
            <v-col>
              <v-row>
                <br />
                <v-card-title class="tituloc">
                  {{ customer.name }}
                </v-card-title>
              </v-row>
              <v-row>
                <v-btn
                  class="btnCardv"
                  variant="text"
                  block
                  @click="openDialog(customer)"
                >
                  Ver cliente
                </v-btn>
              </v-row>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
      <v-card v-if="dialog" class="infoC" elevation="7">
        <v-dialog
          v-model="dialog"
          width="auto"
          style="background: none !important; border-radius: 80px !important"
        >
          <DescripcionC
            :customer="currentCustomer"
            @closeDialog="closeDialog"
          ></DescripcionC>
          <v-btn class="btnCerrarC" @click="dialog = false" icon elevation="0"
            ><v-icon class="iconC">mdi-close</v-icon></v-btn
          >
        </v-dialog>
      </v-card>
      <v-btn
        fab
        dark
        large
        color="#5995fd"
        class="btn-flotante"
        to="/registrarCliente"
      >
        <v-icon size="50">mdi-plus</v-icon>
      </v-btn>
    </v-row>
  </v-container>
</template>
<script setup>
import axios from "axios";
import DescripcionC from "~/components/cliente.vue";
import * as config from "../config/default.json";
import { getHeaders } from "~/src/auth/jwt.js";

const customers = ref([]);
const dialog = ref(false);
const currentCustomer = ref(null);

const getCustomers = async () => {
  const url = `${config.api_host}/customers`;
  const token = localStorage.getItem("token")
  const headers = getHeaders(token);
  const { data } = await axios.get(url, { headers });
  customers.value = data.info
};
onBeforeMount(() => {
  getCustomers();
});
const closeDialog = () => {
  dialog.value = false;
  getCustomers();
};
watch(dialog, (newValue) => {
  if (!newValue) {
    dialog.value = false;
    getCustomers();
  }
});
const openDialog = (customer) => {
  currentCustomer.value = customer;
  dialog.value = true;
};
</script>

<style>
.tituloc {
  font-family: sans-serif !important;
  font-weight: bold !important;
  text-align: center !important;
  font-size: 20px !important;
  margin-left: 10px !important;
  margin-top: 10px !important;
}

.btnCardv {
  color: #5995fd !important;
  margin-right: 25px !important;
  margin-top: 10px;
  margin-bottom: 20px;
}

.customerCard {
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

.btnCerrarC {
  position: absolute;
  top: 8%;
  right: 1%;
  outline: none !important;
  border: 0 !important;
}
</style>
