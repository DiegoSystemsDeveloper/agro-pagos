<template>
  <div class="p-5">
    <b-form @submit="onSubmit" @reset="onReset" v-if="show">
      <b-form-group id="input-group-1" label="Nombre *" label-for="input-1">
        <b-form-input
          id="input-1"
          v-model="form.name"
          required
          placeholder="Nombre"
        ></b-form-input>
      </b-form-group>
      
      <b-form-group id="input-group-2" label="Apellidos *" label-for="input-2">
        <b-form-input
          id="input-2"
          v-model="form.lastName"
          required
          placeholder="Apellidos"
        ></b-form-input>
      </b-form-group>

      <b-form-group
        id="input-group-5"
        label="Email address *"
        label-for="input-5"
        description="Nunca compartas tu email con nadie."
      >
        <b-form-input
          id="input-5"
          v-model="form.email"
          type="email"
          required
          placeholder="Enter email"
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-6" label="Contraseña *" label-for="input-6">
        <b-form-input
          id="input-6"
          v-model="form.password"
          type="password"
          required
          placeholder="Contraseña"
        ></b-form-input>
      </b-form-group>

        <b-button v-show="form.update" type="submit" variant="primary" @submit="onSubmit()">Actualizar</b-button>
        <b-button v-show="true" variant="danger" @click="modeloEliminar = true">Eliminar</b-button>
      <b-modal 
        ref="modeloActualizar" 
        size="sm" 
        title="Mensaje" 
        ok-only
        ><p class="my-1">Usuario actualizado</p>
      </b-modal>

      <b-modal 
        ref="modeloEliminar" 
        size="sm" 
        title="Mensaje" 
        ok-only
        ><p class="my-1">Usuario eliminado</p>
      </b-modal>

       <b-modal id="my-modal" size="sm" title="Eliminar" v-model="modeloEliminar" hide-footer>
            <div class="d-block">¿Desea eliminar la oferta seleccionada?</div>
            <br>
            <b-button @click="eliminarUsuario" variant="success">Si</b-button>
            <b-button @click="modeloEliminar = false"  variant="danger" >No</b-button>
        </b-modal>

               

    </b-form>
    
  </div> 
</template>

<script> 

import { isNull } from 'util';
import config from "../assets/config";
const url_api = config.API_URL;

  export default {
    name: 'Agricultor',
    data() {
      return {
        form: {
          id:'',
          email: '',
          lastName: '',
          name: '',
          
          password: '',
          update: true
        },

        modeloEliminar: false,
        modeloEliminado: false,

        nombre:"",
        
        show: true,
        items: [],
        fields: [
          { key: 'name', label: 'Nombre'},
          { key: 'lastName', label: 'Apellido'},
          { key: 'email', label: 'E-mail'},
          { key: 'actions', label: 'Actions'}
        ],
      }
    },

    mounted: async function(){
        let usuario = JSON.parse(localStorage.getItem('userIn'))
        this.id_usuario = usuario.id_usuario;
        let url = url_api + 'usuarios/' + usuario.id_usuario;
        let {data} = await this.$axios.get(url);

        this.form.name = data.info[0].nombre;
        this.form.lastName = data.info[0].apellido;
        this.form.email = data.info[0].correo;
        this.form.password= data.info[0].contraseña;
        
        console.log(data);
    },

    methods: {

        async eliminarUsuario(){
            let url = url_api + 'usuarios/' + this.id_usuario;
            let {data} = await this.$axios.delete(url);
            console.log(data);
            this.$router.push("/");
        },

        onSubmit(evt) {

            evt.preventDefault()
        
        },

    

      onReset(evt) {
        evt.preventDefault()
        // Reset our form values
        this.clear()
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      },existe(id){
        for(let index = 0; index < this.items.length; index++){
          if(id == this.items[index].id){
            return true
          }
        }
      },deleteUser(id){
        for(let index = 0; index < this.items.length; index++){
          if(id == this.items[index].id){
            this.items.splice(index,1)
          }
        }
        this.$refs['modeloEliminar'].show()
      },updateUser2(){
        for(let index = 0; index < this.items.length; index++){
          if(this.items[index].update){
            this.items[index].email = this.form.email
            this.items[index].name = this.form.name
            this.items[index].lastName = this.form.lastName
            this.items[index].rol = this.form.rol
            this.items[index].password = this.form.password
            this.form.update = false
            this.items[index].update = false
            this.clear()
          }
        }
        this.$refs['modeloActualizar'].show()
      },cancelUpdate(){
        this.form.update = false
        this.clear()
        for(let index = 0; index < this.items.length; index++){
          this.items[index].update = false
        }
      },clear(){
        this.form.email = ''
        this.form.name = ''
        this.form.password = ''
        this.form.lastName = ''
        this.form.id = ''
        this.form.rol = null
      }
    }
  }
</script>