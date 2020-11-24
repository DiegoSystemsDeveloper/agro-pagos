<template>
  <div>
    <Navegacion />
    <Ofertas />
  </div>
</template>

<script>
import Navegacion from '../components/NavComprador'
import Ofertas from '../components/OfertaEnviadas'
export default {
  components: {
    Navegacion,
    Ofertas
  },
};
</script>