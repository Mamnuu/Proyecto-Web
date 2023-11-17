<template>
  <v-layout v-if="!loading" >
    <v-navigation-drawer expand-on-hover rail >
      <v-list>
        <v-list-item prepend-avatar="https://randomuser.me/api/portraits/women/85.jpg" :title="user.name"
          :subtitle="user.email"></v-list-item>
      </v-list>

      <v-divider></v-divider>

      <v-list density="compact" nav>
        <v-list-item prepend-icon="mdi-food" title="Productos" to="/home" value="productos"></v-list-item>
        <v-list-item prepend-icon="mdi-account-hard-hat" title="Vendedores" to="/vendedores"
          value="vendedores"></v-list-item>
        <v-list-item prepend-icon="mdi-account-tie" title="Proveedores" to="/proveedores"
          value="proveedores"></v-list-item>
        <v-list-item prepend-icon="mdi-account-cash" title="Clientes" to="/clientes" value="clientes"></v-list-item>
        <v-list-item prepend-icon="mdi-logout" title="Logout" value="logout" @click="signout" to="/"></v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main style="height: 250px">
      <slot />
    </v-main>
  </v-layout>
</template>
<script setup>

import axios from 'axios'
import config from '../config/default.json'

const signout =()=>{
  localStorage.removeItem("token")
}
const user = ref({ 'name': 'name', "email": "email" });

const getUser =()=>{
  const name= localStorage.getItem("name");
  const email= localStorage.getItem("email");
  user.value.name = name;
  user.value.email = email;

}
const loading = ref(true)
onBeforeMount(() => {
  console.log(localStorage);
  const name= localStorage.getItem("name");
  const email= localStorage.getItem("email");
  const token = localStorage.getItem('token')
  getUser();
  if (token) {
    loading.value = false
  } else {
    useRouter().push('/')
  }
})


</script>