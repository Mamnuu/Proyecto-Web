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
                <v-img src="./images/imgcliente.jpg" class="imgc"></v-img>
              </v-col>
              <v-col>
                <v-row>
                  <br />
                  <v-card-title class="tituloc">
                    {{ customer.nombre }}
                  </v-card-title>
                </v-row>
                <v-row>
                  <v-btn class="btnCardv" variant="text"> Ver cliente </v-btn>
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
  const customers = ref([]);
  
  const getCustomers = async () => {
    const url = "http://localhost:3001/customers";
    const response = await axios.get(url);
    customers.value = response.data;
  };
  getCustomers();
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
  .imgc{
    height: 100px !important;
  }
  </style>
  