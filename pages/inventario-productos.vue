<template>
  <div>
    <Nav2 />
    <Prod />
  </div>
</template>

<script>
import { log } from 'util';
import Navegacion from "../components/Nav2";

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
