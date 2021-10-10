<template>
   <v-app id="login">
      <v-main v-if="showLogin">
         <v-container fluid fill-height>
            <v-layout align-center justify-center>
               <v-flex xs12 sm8 md6 lg4 xl2>
                  <v-card class="elevation-12">
                     <v-toolbar dark color="primary">
                        <v-toolbar-title>Mascotas</v-toolbar-title>
                     </v-toolbar>
                     <v-card-text>
                        <v-form>
                           <v-text-field
                           v-model="emailLogin"
                             prepend-icon="mail"
                              name="login"
                              placeholder="Correo"
                              type="text"
                              id="emailLogin"
                           ></v-text-field>
                           <v-text-field
                           v-model="passwordLogin"
                              prepend-icon="lock"
                              name="password"
                              placeholder="Contraseña"
                               id="passwordLogin"
                           ></v-text-field>
                        </v-form>
                     </v-card-text>
                     <v-card-actions>
                          <v-btn color="purple lighten-1" text  @click="showRegisterForm()">Registrarse</v-btn>
                        <v-spacer></v-spacer>
                        <v-btn color="primary" :disabled="!canLogin" to="/">Ingresar</v-btn>
                     </v-card-actions>
                  </v-card>
               </v-flex>
            </v-layout>
         </v-container>
      </v-main>

       <v-main v-if="!showLogin">
         <v-container fluid fill-height>
            <v-layout align-center justify-center>
               <v-flex xs12 sm8 md6 lg4 xl2>
                  <v-card class="elevation-12">
                     <v-toolbar dark color="primary">
                        <v-toolbar-title>Registro de Usuarios</v-toolbar-title>
                     </v-toolbar>
                     <v-card-text>
                        <v-form>
                             <v-text-field
                            v-model="emailRegister"
                            id="emailRegister"
                              prepend-icon="mail"
                              placeholder="Correo"
                              required
                              :rules="rules.mail"
                           ></v-text-field>

                           <v-text-field
                           v-model="passwordRegister"
                           id="passwordRegister"
                              prepend-icon="lock"
                              placeholder="Contraseña"
                              type="password"
                           ></v-text-field>
                         
                        </v-form>
                     </v-card-text>
                     <v-card-actions>
                          <v-btn color="red lighten-2" text   @click="showLoginForm()">Volver</v-btn>
                        <v-spacer></v-spacer>
                          <v-btn dark color="green lighten" :disabled="!canRegister" @click="makeRegister()">Registrarse</v-btn>
                     </v-card-actions>
                  </v-card>
               </v-flex>
            </v-layout>
         </v-container>
      </v-main>

        <v-dialog v-model="isLoading" persistent width="300" >
            <v-card color="primary" dark >
            <v-card-text> 
                Espere un moemnto por favor 
                <v-progress-linear indeterminate color="white" class="mb-0" ></v-progress-linear>
            </v-card-text>
            </v-card>
            </v-dialog>
        
   </v-app>
</template>

<script>
import {axios} from "axios";

export default {
    name: 'Login',
    data: () => ({
        emailLogin:'',
        passwordLogin:'',
        emailRegister:'',
        passwordRegister:'',
        showLogin: true,
        showRegistro: false,
        isLoading:false,
        rules:{
            mail: [
            val => (val || '').length > 0 && (/@gmail.com\s*$/.test(val)) || 'Solo Correos Gmail',
            ],
        },
    }),
    computed:{
        canLogin(){
            return(
                this.passwordLogin != '' &&
                this.emailLogin != '' 
            ); 
        },
        canRegister(){
            return(
                this.passwordRegister != '' &&
                this.emailRegister != '' 
            ); 
        },
    },
    props: {
        source: String,
    },
    methods:{
        showRegisterForm() {
            this.showLogin = false;
            this.emailLogin = '';
            this.passwordLogin = '';
        },
        showLoginForm() {
             this.showLogin = true;
            this.emailRegister = '';
            this.passwordRegister = '';
        },
        async makeRegister(){
            this.isLoading = true;
            console.log(this.emailRegister);
            setTimeout(() => (this.isLoading = false), 4000)
        }
    },
}
</script>