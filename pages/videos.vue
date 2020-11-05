<template>
  <div>
    <Navegacion />
    <Videos />
  </div>
</template>

<script>
import Navegacion from '../components/Nav2'
import Videos from '../components/crudVideos'
export default {
  components: {
    Navegacion,
    Videos
  },
};
</script>