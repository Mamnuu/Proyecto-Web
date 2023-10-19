<template>
  <v-layout>
    <v-navigation-drawer expand-on-hover rail>
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
        <v-list-item prepend-icon="mdi-logout" title="Logout" to="/" value="logout"></v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main style="height: 250px">
      <slot />
    </v-main>
  </v-layout>
</template>
<script setup>
const user = ref({ 'id': 'usuario', "email": "email" });
let stringUser = sessionStorage.getItem('USER');
if (stringUser) {
  let json = JSON.parse(stringUser);
  user.value = json;
}
</script>