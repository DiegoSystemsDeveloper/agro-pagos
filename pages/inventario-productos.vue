<template>
  <div class="container" id="inventario">
    <Navegacion />
    <div class="row mt-5">
      <div class="col-sm-6">
        <h1>Mis productos publicados</h1>
        <br />
        <div v-for="(producto, index) in productos" :key="index">
          <Producto
            :id="producto.id"
            :nombre="producto.nombre"
            :precio="producto.precio"
            :fecha="producto.fecha"
            :descripcion="producto.descripcion"
            @eliminar="eliminar"
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
            <form>
              <div class="form-group">
                <input
                  type="text"
                  class="form-control"
                  placeholder="Nombre del producto"
                  v-model="nombre"
                  required
                />
              </div>
              <div class="form-group">
                <input
                  type="number"
                  class="form-control"
                  placeholder="Precio base"
                  v-model="precio"
                  required
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
                  v-model="descripcion"
                  required
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
import { v4 as uuidv4 } from "uuid";

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
      nombre: "",
      precio: "",
      fecha: "",
      descripcion: ""
    };
  },
  methods: {
    guardar() {
      if (
        this.nombre.trim() === "" ||
        this.precio.trim() === "" ||
        this.descripcion.trim() === ""
      ) {
        alert("faltan campos por llenar");
      } else {
        this.productos.push({
          id: uuidv4(),
          nombre: this.nombre,
          precio: this.precio,
          fecha: new Date(),
          descripcion: this.descripcion
        });
        (this.nombre = ""),
          (this.precio = ""),
          (this.fecha = ""),
          (this.descripcion = "");
      }
    },
    eliminar(id) {
      let newProductos = this.productos.filter(producto => producto.id !== id);
      this.productos = newProductos;
    }
  }
};
</script>
