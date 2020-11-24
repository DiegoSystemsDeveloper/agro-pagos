<template>
  <div>
    <Navegacion />
    <Directorio />
  </div>
</template>

<script>

import Directorio from '../components/directorio'
import Navegacion from "../components/Navegacion";
export default {
  components: {
    Directorio,
    Navegacion
  },
};
</script>