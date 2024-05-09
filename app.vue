<template>
  <div>
    <v-app>
      <template>
  <v-card>
    <v-layout>
      <v-app-bar
        color="primary"
        prominent
      >
        <v-app-bar-nav-icon variant="text" @click.stop="drawer = !drawer"></v-app-bar-nav-icon>

        <v-toolbar-title>Menu</v-toolbar-title>
       

        <v-spacer></v-spacer>
        
      </v-app-bar>
      <v-navigation-drawer
        v-model="drawer"
        image="https://cdn.vuetifyjs.com/images/backgrounds/bg-2.jpg"
        theme="dark"
        permanent
      >
        <v-list nav>
          <v-list-item prepend-icon="mdi-lock" title="Additive cipher" value="Additive cipher" to="/"></v-list-item>
          <v-list-item prepend-icon="mdi-lock" title="Multiplicative cipher" value="Multiplicative cipher " to="/multiplicative"></v-list-item>
          <v-list-item prepend-icon="mdi-lock" title="Affine cipher" value="Affine" to="/affine"></v-list-item>
          <v-list-item prepend-icon="mdi-lock" title="Autokey cipher" value="Autokey" to="/autokey"></v-list-item>
        </v-list>
      </v-navigation-drawer>
      <v-main style="height: 100vh">
        <NuxtPage />
      </v-main>
    </v-layout>
  </v-card>
</template>
     
    </v-app>
  </div>
</template>

<script setup>

const drawer = ref(true);


</script>
