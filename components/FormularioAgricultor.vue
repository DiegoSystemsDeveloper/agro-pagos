<template>
  <div class="p-5">
    <h1>Registro</h1>
    <b-form @submit="onSubmit" @reset="onReset" v-if="show">
    
      <b-form-group id="input-group-1" label="Nombre:" label-for="input-1">
        <b-form-input
          id="input-1"
          v-model="form.name"
          required
          placeholder="Nombre"
        ></b-form-input>
      </b-form-group>
      
      <b-form-group id="input-group-2" label="Apellidos:" label-for="input-2">
        <b-form-input
          id="input-2"
          v-model="form.lastName"
          required
          placeholder="Apellidos"
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-3" label="Cedula:" label-for="input-3" v-show="form.update == false">
        <b-form-input
          id="input-3"
          v-model="form.id"
          type="number"
          required
          placeholder="Cedula"
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-4" label="Ocupación:" label-for="input-4">
        <b-form-select
          id="input-4"
          v-model="form.ocupation"
          :options="ocupation"
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

      <b-form-group id="input-group-6" label="Contraseña:" label-for="input-6">
        <b-form-input
          id="input-6"
          v-model="form.password"
          type="password"
          required
          placeholder="Contraseña"
        ></b-form-input>
      </b-form-group>


      <b-button v-show="form.update == false" type="submit" variant="primary">Aceptar</b-button>
      <b-button v-show="form.update == false" type="reset" variant="danger">Cancelar</b-button>
      <b-button v-show="form.update" @click="updateUser2">Actualizar</b-button>
      <b-button v-show="form.update" @click="cancelUpdate">Cancelar</b-button>
    </b-form>
    <br>
    <b-table striped hover :items="items" :fields="fields" :tbody-tr-class="rowClass">
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
  export default {
    name: 'Agricultor',
    data() {
      return {
        form: {
          id:'',
          email: '',
          lastName: '',
          name: '',
          ocupation: null,
          password: '',
          update:false
        },
        ocupation: [{ text: 'Selecciona una', value: null }, 'Agricultor', 'Transportista'],
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
    methods: {
      rowClass(item){
        if(!item.update ) return 'table-light'
        if(item.update ) return 'table-warning'
      },onSubmit(evt) {
        evt.preventDefault()
        //alert(JSON.stringify(this.form))
        if(!this.existe(this.form.id)){
          this.items.push(
            {
              id:this.form.id,
              name:this.form.name,
              lastName:this.form.lastName,
              email:this.form.email,
              ocupation:this.form.ocupation,
              password:this.form.password,
              update:false
            }
          )
        }
        //this.con++
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
      },updateUser(id){
        for(let index = 0; index < this.items.length; index++){
          if(id == this.items[index].id){
            this.form.email = this.items[index].email
            this.form.name = this.items[index].name
            this.form.lastName = this.items[index].lastName
            this.form.ocupation = this.items[index].ocupation
            this.form.password = this.items[index].password
            this.form.update = true
            this.items[index].update = true
          }
        }
      },updateUser2(){
        for(let index = 0; index < this.items.length; index++){
          if(this.items[index].update){
            this.items[index].email = this.form.email
            this.items[index].name = this.form.name
            this.items[index].lastName = this.form.lastName
            this.items[index].ocupation = this.form.ocupation
            this.items[index].password = this.form.password
            this.form.update = false
            this.items[index].update = false
            this.clear()
          }
        }
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
        this.form.ocupation = null
      }
    }
  }
</script>