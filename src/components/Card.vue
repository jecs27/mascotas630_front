<template>
  <v-card class="mx-auto" max-width="344">
    
    <v-card-title>Coco </v-card-title>
    <v-card-subtitle> Labrador </v-card-subtitle>
    
    <v-card-actions>
      <v-btn icon @click="show = !show" >
        <v-icon>{{ show ? 'keyboard_arrow_up' : 'keyboard_arrow_down' }}</v-icon>
      </v-btn>
      <v-spacer></v-spacer>
      <v-btn color="green lighten-2" text >Editar</v-btn>
    </v-card-actions>

    <v-expand-transition>
      <div v-show="show">
        <v-divider></v-divider>
        <img src="data:image/jpeg;base64," width="126px" >
      </div>
    </v-expand-transition>
  </v-card>
</template>


<script>
  export default {
    data: () => ({
      show: false,
    }),
  }
</script>