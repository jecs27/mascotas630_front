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
                              type="password"
                           ></v-text-field>
                        </v-form>
                     </v-card-text>
                     <v-card-actions>
                          <v-btn color="purple lighten-1" text  @click="showRegisterForm()">Registrarse</v-btn>
                        <v-spacer></v-spacer>
                        <v-btn color="primary" :disabled="!canLogin" @click="login()">Ingresar</v-btn>
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
        

        

   <v-dialog
      v-model="showDialogMessage"
      persistent
      max-width="290"
    >
      <v-card>
        <v-card-title class="text-h5">
          {{this.dialogMessageTitle}}
        </v-card-title>
        <v-card-text>{{this.dialogMessage}}</v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="green darken-1"
            text
            @click="hideDialogMessage()"
          >
            Aceptar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>


   </v-app>
</template>

<script>
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
        showDialogMessage:false,
        dialogMessageTitle:'',
        dialogMessage:'',

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
           if(this.validatePassword()){
               this.isLoading = true;
               let databody = {
                     email: this.emailRegister,
                     password: this.passwordRegister
               };

               await this.axios.post('http://mascotaseistreinta.ml/users/register', databody)
               .then((result) => {
               console.log(result);  
               this.isLoading = false
               }).catch((err) => {
                  this.isLoading = false
                  this.dialogMessageTitle = 'Error al Registrarse';
                  this.dialogMessage = err.response.data.message;
                  this.showDialogMessage = true;
               });
           } else {
              this.dialogMessageTitle = 'Contraseña Invalida';
               this.dialogMessage = 'La contraseña debe contar con al menos 8 caracteres, un caracter especial, un número y una mayúscula.';
               this.showDialogMessage = true;
           }
             
        },
        async login(){
           this.isLoading = true;
               let databody = {
                     email: this.emailLogin,
                     password: this.passwordLogin
               };
               await this.axios.post('http://mascotaseistreinta.ml/users/loginUser', databody)
               .then((result) => {
                  let token_pet = result.data.token;
                  let cookies_user_id = result.data.data.user_id
                  this.$cookies.set( "token_pet",token_pet);
                  this.$cookies.set( "cookies_user_id",cookies_user_id);

                  this.isLoading = false
                  this.$router.push("/");
               }).catch((err) => {
                  this.isLoading = false
                  this.dialogMessageTitle = 'Error al Iniciar Sesión';
                  let messageBody = '';
                  try{
                     messageBody = err.response.data.message;
                  }catch(error){
                     this.dialogMessage = err;
                  }                  
                  this.showDialogMessage = true;
               });
        },
        validatePassword(){
           let re = /^(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9])(?=.*[!@#\$%\^&\*])(?=.{8,})/;
            return re.test(this.passwordRegister) 
        },
        hideDialogMessage(){
            this.dialogMessageTitle = '';
            this.dialogMessage = '';
            this.showDialogMessage = false;
        },
    },
      beforeCreate() {
      let token_pet = this.$cookies.get("token_pet");
      if(token_pet != null){
        this.$router.push("/");
      }
    },
}
</script>