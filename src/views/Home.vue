
<template>

<v-container grid-list-md >
  
       <v-app-bar app color="primary" dark >
      <div class="d-flex align-center">
      </div>
      <v-btn target="_blank" text @click="registerPet()">
        <span class="mr-2">Alta Mascota</span>
        <v-icon>pets</v-icon>
      </v-btn>
      <v-spacer></v-spacer>
      <v-btn target="_blank" text @click="logOut()">
        <span class="mr-2">Cerrar Sesión</span>
        <v-icon>logout</v-icon>
      </v-btn>
    </v-app-bar>  


  <v-layout row wrap>
    <v-main v-for="(pet, index) in petList" :key="index">
      <v-flex >
      <v-card width="350" max-width="600">
        <v-card-title>{{pet.name}} </v-card-title>
        <v-card-subtitle> {{pet.breed}} </v-card-subtitle>
        <v-card-actions>
          <v-btn color="red lighten-1" text  @click="deletePet(index)">Borrar</v-btn>
          <v-spacer></v-spacer>
          <v-btn color="green lighten-2" text  @click="editPet(index)">Editar</v-btn>
        </v-card-actions>            
      </v-card>
    </v-flex>
    </v-main>

  </v-layout>



  <template>
  <v-row justify="center">
    <v-dialog
      v-model="dialog"
      persistent
      max-width="700"
    >
      <v-card>
        <v-card-title>
          <span class="text-h5">Editar de Mascotas</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col
                cols="12"
                sm="6"
              >
                <v-text-field
                v-model="petName"
                  label="Nombre de la mascota*"
                  required
                ></v-text-field>
              </v-col>
        
              <v-col  cols="12"
                sm="6">
                <v-text-field
                v-model="petBreed"
                  label="Raza*"
                  required
                ></v-text-field>
              </v-col>
            </v-row>
            <v-row>
              <v-col v-for="img in pet_data" :key="img.petImage_id">
               <v-img
                  :src="`data:image/jpeg;base64,${img.image}`"
                  height="200px"
                  width="200px"
                ></v-img>
                
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



<template>
  <v-row justify="center">
    <v-dialog
      v-model="registerDialog"
      persistent
       width="700" 
      max-width="900"
    >
      <v-card>
        <v-card-title>
          <span class="text-h5">Registro de Mascotas</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12" sm="6">
                <v-text-field v-model="petName" label="Nombre de la mascota*" required ></v-text-field>
              </v-col>
        
              <v-col  cols="12" sm="6">
                <v-text-field v-model="petBreed" label="Raza*" required
                ></v-text-field>
              </v-col>
            </v-row>

            <v-row>
            <v-col cols="12" sm="6">
            <v-menu
              ref="menu"
              :close-on-content-click="false"
              :return-value.sync="petBirthday"
              transition="scale-transition"
              offset-y
              min-width="auto"
            >
              <template v-slot:activator="{ on, attrs }">
                <v-text-field
                  v-model="petBirthday"
                  label="Fecha Nacimiento"
                  prepend-icon="mdi-calendar"
                  readonly
                  v-bind="attrs"
                  v-on="on"
                ></v-text-field>
              </template>
              <v-date-picker
                v-model="petBirthday"
                no-title
                scrollable
                :max="maxDate"
              >
                <v-spacer></v-spacer>
                <v-btn
                  text
                  color="primary"
                  @click="$refs.menu.save(petBirthday)"
                >
                  OK
                </v-btn>
              </v-date-picker>
            </v-menu>
          </v-col>


          
          <v-col cols="12" sm="6">
            <v-file-input
              v-model="images"
              multiple
              label="Imágenes"
              clearable
            ></v-file-input>
          </v-col>
        </v-row>

          </v-container>
         
        </v-card-text>
      
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="red darken-1" text @click="registerDialog = false" >
            Cancelar
          </v-btn>
          <v-btn color="green darken-1" text @click="makeRegisterPet()" >
            Registrar
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
      registerDialog:false,
      show: false,
      dialog: false,
      nLimitData:12,
      petList:[],
      petName:'',
      petBreed:'',
      pet_id_dialog:0,
      maxDate: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
      petBirthday:'',
      images:[],
      pet_data:[],
    }),
    components:{
      
    },
    methods:{
      editPet(pet_id){

        this.petName = this.petList[pet_id].name;
        this.petBreed = this.petList[pet_id].breed;
        this.pet_data = this.petList[pet_id].images;
 
        this.dialog = true
      },
      logOut(){
        this.$cookies.remove("token_pet");
        this.$cookies.remove("cookies_user_id");
        this.$router.push("/login");
      },
      async getPetList(){

        let databody = {
            lastId: 0,
            limitData: this.nLimitData,
            user_id: this.$cookies.get("cookies_user_id")
        };
        await this.axios.post('http://mascotaseistreinta.ml/pets/listMyPets', databody,{
          headers: {
            Authorization: 'Bearer ' + this.$cookies.get("token_pet")
          }
        })
        .then((result) => {
          this.petList = result.data.data.petListData.rows;
        }).catch((err) => {
          console.log(err.response);
        });
      },
      async registerPet(){
        this.registerDialog = true;
      },
      async makeRegisterPet(){

        if(this.petName != '' && 
          this.petBreed != '' &&
          this.petBirthday != ''
        ){ 
          this.registerDialog = false;
          const form = new FormData();
          form.append('user_id', this.$cookies.get("cookies_user_id"));
          form.append('name',  this.petName );
          form.append('breed', this.petBreed);
          form.append('birthday', this.petBirthday);

          if(this.images.length > 0) {
            for(let imgP of this.images){
              form.append('image', imgP);
            }
          }
          await this.axios({
              method: 'post',
              url: 'http://mascotaseistreinta.ml/pets/registerPet',
              data: form,
              headers: {
                  'Content-Type': `multipart/form-data; boundary=${form._boundary}`,
                  'Accept':'*/*',
                  'Authorization': 'Bearer ' + this.$cookies.get("token_pet")
              },
          }) 
          .then((result) => {
            this.getPetList();
          }).catch((err) => {
            console.log(err);
          });
        }
      },
      async deletePet(pet_id){
         await this.axios.delete('http://mascotaseistreinta.ml/pets/deletePet/'+this.petList[pet_id].uuid,{
         headers: {
            Authorization: 'Bearer ' + this.$cookies.get("token_pet")
          }
        },
         )
          .then((result) => {
              this.getPetList();
          }).catch((err) => {
          });
      }
    },
    created() {
     this.getPetList();
    },
    beforeCreate() {
      let cookie_token_pet = this.$cookies.get("token_pet");
      if(cookie_token_pet == null){
        this.$router.push("/login");
      }
     
    },
  }
</script>
