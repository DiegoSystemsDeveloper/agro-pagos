<template>
  <div>
    <Navegacion />
    <Prod />
  </div>
</template>

<script>
import { log } from 'util';
import Navegacion from "../components/NavAgricultor";

import Prod from "../components/ProdAgricultor";
export default {
  components: {
    Navegacion,
    Prod
  },
  name: "inventario",

  data() {
    return {

    };
  },

  
};
</script>
