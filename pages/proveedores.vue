<template>
  <v-container class="container-p">
    <h3 class="titulo">Proveedores</h3>
    <br />
    <v-row>
      <v-col
        v-for="supplier in suppliers"
        :key="supplier.id"
        cols="12"
        md="4"
        sm="6"
      >
        <v-card
          class="supplierCard"
          max-width="300"
          max-height="200"
          elevation="3"
        >
          <v-row>
            <v-col>
              <v-img :src="supplier.img" class="imgc"></v-img>
            </v-col>
            <v-col>
              <v-row>
                <br />
                <v-card-title class="titulop">
                  {{ supplier.nombre }}
                </v-card-title>
              </v-row>
              <v-row>
                <v-btn class="btnCardp" variant="text"> Ver proveedor </v-btn>
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
const suppliers = ref([]);

const getSuppliers = async () => {
  const url = "http://localhost:3001/suppliers";
  const response = await axios.get(url);
  suppliers.value = response.data;
};
getSuppliers();
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
.imgc {
  height: 100px !important;
}
</style>
