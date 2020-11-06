<template>
  <div class="container" id="inventario">
    <Navegacion />
    <div class="row mt-5">
      <div class="col-sm-6">
        <h1>Mis productos publicados</h1>
        <br />
        <div v-for="(producto, index) in productos" :key="index">
          <Producto
            :nombre="producto.nombre"
            :precio="producto.precio"
            :fecha="producto.fecha"
            :descripcion="producto.descripcion"
          />
          <br />
        </div>
      </div>
      <div class="col-sm-6">
        <div class="card">
          <div class="card-header bg-dark text-white text-center">
            <h3>{{ titulo }}</h3>
          </div>
          <div class="card-body">
            <form action="">
              <div class="form-group">
                <input
                  type="text"
                  class="form-control"
                  placeholder="Nombre del producto"
                  value= {newProducto.nombre}
                />
              </div>
              <div class="form-group">
                <input
                  type="number"
                  class="form-control"
                  placeholder="Precio base"
                  
                />
              </div>
              <div class="form-group">
                <textarea
                  class="form-control"
                  name=""
                  id=""
                  cols="30"
                  rows="10"
                  placeholder="Descripcion"
                  v-model="newProducto.descripcion"
                ></textarea>
              </div>
              <div class="form-group">
                <div class="btn btn-primary btn-block" @click="guardar">
                  Publicar
                </div>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navegacion from "../components/Navegacion";
import Producto from "../components/Producto";

export default {
  components: {
    Navegacion,
    Producto
  },
  name: "inventario",
  data() {
    return {
      titulo: "Crear publicación",
      productos: [],
      newProducto: {
        nombre: "",
        precio: "",
        fecha: new Date(),
        descripcion: ""
      }
    };
  },
  methods: {
    guardar() {
      this.productos = [...this.productos, this.newProducto]
    },
    eliminar() {

    }
  }
};
</script>
