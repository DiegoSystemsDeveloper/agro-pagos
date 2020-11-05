<template>
  <div>
    <b-navbar toggleable="lg" type="dark"  id="nav">
      <b-navbar-brand href="#">Agro Pagos</b-navbar-brand>
 
      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
       <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
          <b-nav-item href="../inicio" id="item1">Buscar productos</b-nav-item>
          <b-nav-item href="#" id="item1">Contacto</b-nav-item>
          <b-nav-item-dropdown right>
            <!-- Using 'button-content' slot -->
            <template #button-content>
              <em>Usuario</em>
            </template>
            <b-dropdown-item href="#">Perfil</b-dropdown-item>
            <b-dropdown-item href="#">Sign in</b-dropdown-item>
            <b-dropdown-item v-b-modal.modal-1>Registro</b-dropdown-item>
            <b-modal id="modal-1" title="Registro" hide-footer>
             <Agricultor />
            </b-modal>
          </b-nav-item-dropdown>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>

<script>
import Login from "../components/FormularioLogin";
import Agricultor from "../components/FormularioAgricultor"
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

</style>
