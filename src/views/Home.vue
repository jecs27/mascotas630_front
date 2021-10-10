
<template>

<v-container grid-list-sm >
  <v-layout row wrap>
       <v-app-bar app color="primary" dark >
      <div class="d-flex align-center">
      
      </div>

      <v-spacer></v-spacer>
      <v-btn target="_blank" text>
        <span class="mr-2">Cerrar Sesión</span>
        <v-icon>logout</v-icon>
      </v-btn>
    </v-app-bar>

  <v-flex xs12 sm6 md4 >
    <v-card class="mx-auto" max-width="344">
      <v-card-title>Coco </v-card-title>
      <v-card-subtitle> Labrador </v-card-subtitle>
      <v-card-actions>
        <v-btn icon @click="show = !show" >
          <v-icon>{{ show ? 'keyboard_arrow_up' : 'keyboard_arrow_down' }}</v-icon>
        </v-btn>
        <v-spacer></v-spacer>
         
        <v-btn color="green lighten-2" text  @click="mostrarModal()">Editar</v-btn>
      </v-card-actions>

      <v-expand-transition>
        <div v-show="show">
          <v-divider></v-divider>
          <img src="data:image/jpeg;base64," width="126px" >
        </div>
      </v-expand-transition>
    </v-card>
    </v-flex>
  </v-layout>



  <template>
  <v-row justify="center">
    <v-dialog
      v-model="dialog"
      persistent
      max-width="600px"
    >
      <v-card>
        <v-card-title>
          <span class="text-h5">Registro de Mascotas</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col
                cols="12"
                sm="6"
              >
                <v-text-field
                  label="Nombre de la mascota*"
                  required
                ></v-text-field>
              </v-col>
        
              <v-col  cols="12"
                sm="6">
                <v-text-field
                  label="Raza*"
                  required
                ></v-text-field>
              </v-col>

            </v-row>
          </v-container>
          <small style="color:red">*Campos Requeridos</small>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="red darken-1"
            text
            @click="dialog = false"
          >
            Cancelar
          </v-btn>
          <v-btn
            color="green darken-1"
            text
            @click="dialog = false"
          >
            Guardar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>
</v-container>





</template>


<script>
  export default {
    data: () => ({
      show: false,
      dialog: false
    }),
    components:{
      
    },
    methods:{
      mostrarModal(){
        console.log("entra");
        this.dialog = true
      }
    }
  }
</script>
