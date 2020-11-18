<template>

  <div>
      <h1>Hola usuariooooo</h1>
      <p>{{user}}</p>
  </div>
  
</template>

<script>
import config from "../assets/config";
const url_api = config.API_URL;
export default {
  data() {
    return {
      user: null,
      drawer: null,
      
    };
  },
  beforeMount() {
    this.loadUser();
  },
  methods: {
    async loadUser() {
      try {
        let token = localStorage.getItem("Token");
        let url = url_api + "validacion";
        this.$axios.setToken(token, "Bearer");
        let { data } = await this.$axios.get(url);
        this.user = data.info;
        localStorage.setItem("userIn", JSON.stringify(data.info));
        console.log(this.user);
      } catch (error) {
        this.$router.push("/");
      }
    },

    logout() {
      localStorage.removeItem("token");
      localStorage.removeItem("userIn");
      this.$router.push("/");
    },
  },
};
</script>