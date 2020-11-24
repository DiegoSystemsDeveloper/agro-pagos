<template>
  <b-container>
          
    <b-form @submit="login">
      <b-form-group
        id="input-group-email"
        label="Dirección de correo electrónico: *"
        label-for="input-email"
      >
      <div>
        <b-form-input
          id="input-email"
          type="email"
          v-model="email"
          placeholder="Ingrese correo electrónico"
          required
        ></b-form-input>
      </div>
        
      </b-form-group>

      <b-form-group
          id="input-group-password"
          label="Contraseña: *"
          label-for="input-password"
        >
        <b-form-input
          id="input-password"
          type="password"
          v-model="contraseña"
          required
          placeholder="Ingrese su contraseña"
        ></b-form-input>
      </b-form-group>
        
      <b-button block class="mt-4" variant="primary" type="submit"
        >Ingresar</b-button>
      <b-modal 
        v-model="modalUsuario" 
        size="sm" 
        title="Error" 
        ok-only
      ><p class="my-1">Usuario no encontrado. Por favor revise su correo o contraseña</p>
      </b-modal>

    </b-form>
    <div class="text-center mt-2">
      <p>
        <a href="#" class="mt-2">¿Olvidaste tu contraseña?</a>
      </p>
    </div>
  </b-container>
  
</template>

<script>
import config from "../assets/config";
const url_api = config.API_URL;
export default {
  name: 'Login', 
  
  data(){
    return{
      modalUsuario: false,
      email: "",
      contraseña: null,
      user: null,
    }
  },

  methods:{

    async login(evt) {
      this.modalUsuario = false;
      evt.preventDefault();

      if (this.email && this.contraseña) {
        try {
          let url = url_api + 'login';
          let parametros = {};
          parametros.correo = this.email;
          parametros.contraseña = this.contraseña;
          let  {data} = await this.$axios.post(url, parametros);

          if(data.ok){
            this.modalUsuario = false;
            localStorage.setItem('Token', data.info);

            let token = localStorage.getItem("Token");
            url = url_api + "validacion";
            this.$axios.setToken(token, "Bearer");
            let  response  = await this.$axios.get(url);
            this.user = response.data.info;

            localStorage.setItem("userIn", JSON.stringify(response.data.info));

            if(this.user.rol === "Agricultor"){
              this.$router.push("/inventario-productos");
            }else if(this.user.rol === "Comprador"){
              this.$router.push("/productosUsuario");
            }else{
              this.$router.push("/transUsuario");
            }

          }

          console.log(this.user);
          
        } catch (error) {
          if(error.response){
            this.modalUsuario = true;
          }else{
            console.log(error);
          }
        } 
      }
      
    },

  }
}
</script>