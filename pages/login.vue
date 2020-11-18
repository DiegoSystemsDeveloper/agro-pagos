<template>
  <div>
    <Nav2 />
    <Agricultor />
  </div>
</template>
<script>
import Login from "../components/FormularioLogin";
import Nav2 from '../components/Nav2'
import Agricultor from "../components/FormularioAgricultor"
export default {
  components: {
    Login,
    Nav2,
    Agricultor
  },
};
</script>
