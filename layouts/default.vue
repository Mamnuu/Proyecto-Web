<template>
  <v-layout v-if="!loading" >
    <v-navigation-drawer expand-on-hover rail >
      <v-list>
        <v-list-item :prepend-avatar="user.img" :title="user.fullName"
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
const loading = ref(true)
onBeforeMount(() => {
  console.log(localStorage);
  const token = localStorage.getItem('token')
  if (token) {
    loading.value = false
  } else {
    useRouter().push('/')
  }
})

const user = ref({ 'id': 'usuario', "email": "email" });


/*let stringUser = localStorage.getItem('USER');
if (stringUser) {
  let json = JSON.parse(stringUser);
  user.value = json;
}*/

</script>