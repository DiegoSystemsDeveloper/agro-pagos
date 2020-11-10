<template>
  <div class="card">
    <div class="card-header bg-dark text-white text-center">
      <h3>{{nombre}}</h3>
    </div>
    <div class="card-body">
      <form action="">
        <div class="form-group">
          <p>Precio base: <span>$ {{precio}}</span></p>
        </div>
        <div class="form-group">
          <p>Fecha publicacion: <span>{{fecha}}</span></p>
        </div>
        <div class="form-group">
          <p>Descripcion: <span>{{descripcion}}</span></p>
        </div>
        <div class="form-group">
          <div class="row">
            <div class="col-sm-6">
              <div class=" btn btn-danger btn-block" @click="eliminar">
                Eliminar
              </div>
            </div>
            <div class="col-sm-6">
              <div class=" btn btn-secondary btn-block">
                editar
              </div>
            </div>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
    name: 'Producto',
    props: ["id","nombre", "precio", "fecha", "descripcion"],
    methods: {
      eliminar() {
        this.$emit("eliminar", this.id);
      }
    }
}
</script>