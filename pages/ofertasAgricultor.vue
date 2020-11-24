<template>
  <div>
    <Navegacion />
    <OfertasAgri />
  </div>
</template>

<script>
import Navegacion from '../components/NavAgricultor'
import OfertasAgri from '../components/ofertasAgricultor'
export default {
  components: {
    Navegacion,
    OfertasAgri
  },
};
</script>