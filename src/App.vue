<template>
  <v-app>
    <v-app-bar app color="primary" dark >
      <div class="d-flex align-center">
      
      </div>

      <v-spacer></v-spacer>
      <v-btn target="_blank" text>
        <span class="mr-2">Cerrar Sesión</span>
        <v-icon>logout</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  data: () => ({
   
  }),
  
};
</script>
