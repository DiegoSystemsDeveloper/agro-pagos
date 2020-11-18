<template>

  <div>
      <Nav2 />
      <Defecto />
  </div>
  

</template>

<script>
import Navegacion from '../components/Nav2'
import Agricultor from "../components/FormularioAgricultor"
import Ofertas from '../components/CrudOfertas'
import Defecto from '../components/Defecto'
export default {
  components: {
    Navegacion,
    Agricultor,
    Defecto
  },
};
</script>

