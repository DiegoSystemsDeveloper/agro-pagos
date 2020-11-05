<template>
  <Nav2 />
</template>

<script>
import Navegacion from '../components/Nav2'
import Agricultor from "../components/FormularioAgricultor"
import Ofertas from '../components/CrudOfertas'
export default {
  components: {
    Navegacion,
    Agricultor
  },
};
</script>

<style>
body{
  background-color:rgb(219, 217, 217);
  border-radius: 10px;
}
</style>