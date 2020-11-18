<template>
  <div>
    <div class="container" id="inventario">
      <div class="row mt-5">
        
        <div class="col-sm-6">
          <h1 v-if="productos.length > 0">Mis productos publicados</h1>
          <h1 v-else>No tienes productos publicados</h1>  
          <br />
          <div>
           <div v-for="(producto, index) in productos" :key="index">
            <b-card
                title=""
                tag="article"
                style="max-width: 20rem;"
                class="mb-2"
                >
                  <h5 class="text-center">Producto: {{producto.nombre}}</h5>
                  <p>id: {{producto.id}}</p>
                  <p>Precio base: {{producto.precio}} </p>
                  <p>Imagen: {{producto.imagen.name}}</p>
                  <p>Fecha: {{producto.fecha}}</p>
                  <p>Descripcion: {{producto.descripcion}}</p>
                  <b-button variant="danger" size="sm" @click="eliminar(index)" class="mr-1">
                    Eliminar
                  </b-button>
                  <b-button variant="primary" size="sm" class="mr-1">
                    Editar
                  </b-button>
                </b-card>
            </div> 
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
                    v-model="nombre"
                  />
                </div>
                <div class="form-group">
                  <input
                    type="number"
                    class="form-control"
                    placeholder="Precio base"
                    v-model="precio"
                  />
                </div>
                <div class="form-group">
                  <b-form-file
                    v-model="imagen"
                    :state="Boolean(imagen)"
                    placeholder="Choose a file or drop it here..."
                    drop-placeholder="Drop file here..."
                  ></b-form-file>
                  <div class="mt-3">Imagen: {{ imagen ? imagen.name : '' }}</div>
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
                  ></textarea>
                </div>
                <div class="form-group">
                  <div class="btn btn-primary btn-block" @click="agregar">
                    Publicar
                  </div>
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
export default {

  name: 'ProductoAgricultor',
  data(){
    return{
      titulo: "Crear publicación",
      productos:[],
      
      idProductos: "",
      nombre: "",
      precio: "",
      imagen: null,
      fecha: new Date(),
      descripcion: "",   
    }
  },

  mounted: function(){
    let datos = JSON.parse(localStorage.getItem('ProductosAgricultor'))
    if(datos === null){
      this.productos = [];
    }else{
      this.productos = datos; 
    }
  },

  methods: {

    agregar(){
      
      this.productos.push({
        id: this.nombre,
        nombre: this.nombre,
        precio: this.precio,
        imagen: this.imagen.name,
        fecha: this.fecha,
        descripcion: this.descripcion,
        panel: false,
      });

      localStorage.setItem('ProductosAgricultor', JSON.stringify(this.productos));

    },

    eliminar(index){
      this.productos.splice(index,1);
      localStorage.setItem('ProductosAgricultor', JSON.stringify(this.productos));
    }

  },

}


</script>