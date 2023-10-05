<template>
  <v-layout>
    <v-navigation-drawer expand-on-hover rail>
      <v-list>
        <v-list-item
          prepend-avatar="@/pages/images/perfil-del-usuario.png"
          title="Admin"
          subtitle="admin@admin.com"
        ></v-list-item>
      </v-list>

      <v-divider></v-divider>

      <v-list density="compact" nav>
        <v-list-item
          prepend-icon="mdi-food"
          title="Productos"
          value="productos"
        ></v-list-item>
        <v-list-item
          prepend-icon="mdi-account-hard-hat"
          title="Vendedores"
          value="vendedores"
        ></v-list-item>
        <v-list-item
          prepend-icon="mdi-account-tie"
          title="Proveedores"
          value="proveedores"
        ></v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main style="height: 250px">
      <slot />
    </v-main>
  </v-layout>

</template>
