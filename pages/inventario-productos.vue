<template>
  <div class="container" id="inventario">
    <Navegacion />
    <div class="row mt-5">
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
                  v-model="nombreProducto"
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
                ></textarea>
              </div>
              <div class="form-group">
                <button class=" btn btn-primary btn-block" @click="guardar">
                  Publicar
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
      <div class="col-sm-6">
        <h1>Mis productos publicados</h1>
        <br />
        <div class="card">
          <div class="card-header bg-dark text-white text-center">
            <h3>Arroz en polvo</h3>
          </div>
          <div class="card-body">
            <form action="">
              <div class="form-group">
                <p>Precio base: <span>$ 10000</span></p>
              </div>
              <div class="form-group">
                <p>Fecha publicacion: <span>21-10-2020</span></p>
              </div>
              <div class="form-group">
                <p>Descripcion: <span>Hermoso arroz blanco en polvo</span></p>
              </div>
              <div class="form-group">
                <div class="row">
                  <div class="col-sm-6">
                    <button class=" btn btn-danger btn-block">
                      Eliminar
                    </button>
                  </div>
                  <div class="col-sm-6">
                    <button class=" btn btn-secondary btn-block">
                      editar
                    </button>
                  </div>
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

export default {
  components: {
    Navegacion
  },
  name: "inventario",
  data() {
    return {
      titulo: "Crear publicación",
      productos: [],
      nombreProducto: ""
    };
  },
  methods: {
    guardar() {
      alert(this.data.nombreProducto)
    }
  }

};
</script>
