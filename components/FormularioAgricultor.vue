<template>
  <div class="p-5">
    <b-form @submit="onSubmit" @reset="onReset" v-if="show">
      <b-form-group id="input-group-1" label="Nombre: *" label-for="input-1">
        <b-form-input
          id="input-1"
          v-model="form.name"
          required
          placeholder="Nombre"
        ></b-form-input>
      </b-form-group>
      
      <b-form-group id="input-group-2" label="Apellidos: *" label-for="input-2">
        <b-form-input
          id="input-2"
          v-model="form.lastName"
          required
          placeholder="Apellidos"
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-3" label="Cedula: *" label-for="input-3" v-show="form.update == false">
        <b-form-input
          id="input-3"
          v-model="form.id"
          type="number"
          required
          placeholder="Cedula"
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-7" label="Telefono: *" label-for="input-6">
        <b-form-input
          id="input-7"
          v-model="form.telefono"
          type="number"
          required
          placeholder="Telefono"
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-4" label="Rol: *" label-for="input-4">
        <b-form-select
          id="input-4"
          v-model="form.rol"
          :options="rol"
          required
        ></b-form-select>
      </b-form-group>

      <b-form-group
        id="input-group-5"
        label="Email address:"
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

      <b-form-group id="input-group-6" label="Contraseña: *" label-for="input-6">
        <b-form-input
          id="input-6"
          v-model="form.password"
          type="password"
          required
          placeholder="Contraseña"
        ></b-form-input>
      </b-form-group>
      
      <b-button v-show="form.update == false" type="submit" variant="primary">Aceptar</b-button>
      <b-button v-show="form.update == false" type="reset" variant="danger" data-dismiss = "modal">Cancelar</b-button>
      <b-button v-show="form.update" @click="updateUser2">Actualizar</b-button>
      <b-button v-show="form.update" @click="cancelUpdate">Cancelar</b-button>
    </b-form>
    <b-table v-show="false" striped hover :items="items" :fields="fields" :tbody-tr-class="rowClass">
      <template v-slot:cell(actions)="row">
        <b-button :disabled="form.update" variant="success" size="sm" @click="updateUser(row.item.id)">
          Update
        </b-button>
        <b-button :disabled="form.update" variant="danger" size="sm" @click="deleteUser(row.item.id)">
          Delete
        </b-button>
      </template>
    </b-table>
  </div> 
</template>

<script> 
import { log } from 'util';

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
          rol: null,
          password: '',
          telefono: null,
          update:false,

          usuario: []
        },

        rol: [{ text: 'Selecciona una', value: null }, 'Agricultor', 'Transportista', 'Comprador'],
        show: true,
        items: [],
        props: ["estadoTabla"],
        fields: [
          { key: 'name', label: 'Nombre'},
          { key: 'lastName', label: 'Apellido'},
          { key: 'email', label: 'E-mail'},
          { key: 'actions', label: 'Actions'}
        ],
      }
    },
    methods: {
      rowClass(item){
        if(!item.update ) return 'table-light'
        if(item.update ) return 'table-warning'
      },
      
      async onSubmit(evt) {
        evt.preventDefault()

        let url = url_api + 'usuarios';
        let parametros = {};
        parametros.id_usuario = this.form.id;
        parametros.nombre = this.form.name;
        parametros.apellido = this.form.lastName;
        parametros.telefono = this.form.telefono;
        parametros.correo = this.form.email;
        parametros.rol = this.form.rol;
        parametros.contraseña = this.form.password;

        let  {data} = await this.$axios.post(url, parametros);
        console.log(data);
        
        
        localStorage.setItem("userIn", JSON.stringify(data.info));
            
        if(data.info.rol === "Agricultor"){
          this.$router.push("/inventario-productos");
        }else if(data.info.rol === "Comprador"){
          this.$router.push("/productosUsuario");
        }else{
          this.$router.push("/transportista");
        }
       
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
      },
      
      
      clear(){
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