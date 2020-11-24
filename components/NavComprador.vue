<template>
  <div>
    <b-navbar toggleable="lg" type="dark"  id="nav">
      <b-button v-b-toggle.sidebar-variant id="icon"><b-icon-chevron-bar-right></b-icon-chevron-bar-right></b-button>
      <b-sidebar id="sidebar-variant" title="MENÚ" shadow>
        <div class="px-3 py-2">
          <nav >
            <b-nav vertical>
              <a href="../login" @click="hide" id="item2"><b-icon-people-fill></b-icon-people-fill> MI PERFÍL</a>
              <br>
              <a href="../ofertasUsuario" @click="hide" id="item2"><b-icon-bag-fill></b-icon-bag-fill> OFERTAS ENVIADAS</a>
              <br>
              <a href="../productosUsuario" @click="hide" id="item2"><b-icon-basket2-fill></b-icon-basket2-fill> PRODUCTOS</a>
              <br>
            </b-nav>
          </nav>
        </div>
      </b-sidebar>
      <b-navbar-brand href="/">Agro Pagos</b-navbar-brand>
 
      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
       <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
          <b-nav-item-dropdown right>
            <!-- Using 'button-content' slot -->
            <template #button-content>
              <em>Usuario</em>
            </template>
            <b-dropdown-item href="#">Perfil</b-dropdown-item>
            <b-dropdown-item href="../">Sign out</b-dropdown-item>
          </b-nav-item-dropdown>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>

<script>
import Login from "../components/FormularioLogin";
import Agricultor from "../components/FormularioAgricultor"
import Vue from 'vue'
import { BootstrapVue, BootstrapVueIcons } from 'bootstrap-vue'
Vue.use(BootstrapVue)
Vue.use(BootstrapVueIcons)
export default {
  name:'navegacion',
  components: {
    Login,
    Agricultor
  },
};
</script>

<style>

#nav{
  background-color: rgb(15, 14, 14);
  border-radius: 4px;
  margin: 0px 8px;
}

#item1 :hover{
  background-color: rgb(30, 31, 29);
  transform: scale(1.1,1.1);
}

#item2{
  text-decoration: none;
  padding: 1vh;
  position: relative;
  background-color: rgb(241, 239, 239);
  color: black;
}

#icon{
  background-color: rgb(15, 14, 14);
}


</style>