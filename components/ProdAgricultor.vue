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
                  <p>id: {{producto.id_producto}}</p>
                  <p>Precio base: {{producto.precio_base}} </p>
                  <p>Imagen:</p>
                  <p>Fecha: {{producto.fecha}}</p>
                  <p>Descripcion: {{producto.descripcion}}</p>
                  <b-button variant="danger" size="sm" @click="modeloEliminar = true" class="mr-1">
                    Eliminar
                  </b-button>
                  
                </b-card>
                <b-modal id="my-modal" size="sm" title="Eliminar" v-model="modeloEliminar" hide-footer>
                  <div class="d-block">¿Desea eliminar el producto seleccionado?</div>
                  <br>
                  <b-button @click="eliminar(producto.id_producto, index)" variant="success">Si</b-button>
                  <b-button @click="modeloEliminar = false"  variant="danger" >No</b-button>
                </b-modal>
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
                    placeholder="Nombre del producto *"
                    v-model="nombre"
                    
                  />
                    
                </div>
                <div class="form-group">
                  <input
                    type="number"
                    class="form-control"
                    placeholder="Precio base *"
                    v-model="precio"
                  />
                </div>
                <div class="form-group">
                  <b-form-file
                    v-model="imagen"
                    placeholder="Selecciona la imagen del producto"
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
                    placeholder="Descripcion *"
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
    <b-modal 
        v-model="modeloAgregar" 
        size="sm" 
        title="Mensaje" 
        ok-only
    ><p class="my-1">Tu producto ha sido creado</p>
    </b-modal>

    <b-modal 
        v-model="modeloAlerta" 
        size="sm" 
        title="Aviso"
        ok-only
    ><p class="my-1">Debes completar todos los campos</p>
    </b-modal>

    <b-modal 
        v-model="modeloEliminado" 
        size="sm" 
        title="Eliminado" 
        ok-only
    ><p class="my-1">Producto eliminado</p>
    </b-modal>
    <b-modal 
        v-model="modeloActualizado" 
        size="sm" 
        title="Actualizado" 
        ok-only
    ><p class="my-1">Producto actualizada</p>
    </b-modal>

  </div>
</template>


<script>
import { log } from 'util';
import config from "../assets/config";
const url_api = config.API_URL;

export default {

  name: 'ProductoAgricultor',
  data(){
    return{
      titulo: "Crear publicación",
      productos: [],
      
      id_usuario: null,
      idProductos: "",
      nombre: "",
      precio: "",
      imagen: null,
      fecha: new Date(),
      descripcion: "",   

      modeloAgregar: false,
      modeloEliminar:false,
      modeloAlerta: false,
      modeloEliminado: false,
      modeloActualizado: false,

      id_producto: null,
      index: null
    }
  },

  mounted: function(){
    let usuario = JSON.parse(localStorage.getItem('userIn'))
    this.id_usuario = usuario.id_usuario;
    this.getProductos();
  },

  methods: {

    async getProductos(){

      let url = url_api + 'productos/' + this.id_usuario;
      let {data} = await this.$axios.get(url);
      console.log(data.info);
      this.productos = data.info;

    },

   

    async agregar(){

      if((this.nombre , this.precio_base, this.descripcion) != "" ){
        //Asi se guarda en la BDD
      let url = url_api + 'productos';
      let producto = {};
      producto.id_usuario = this.id_usuario;
      producto.nombre = this.nombre;
      producto.precio_base = this.precio;
      producto.fecha = this.fecha;
      producto.descripcion = this.descripcion;
      producto.panel = false;
      
      let  {data} = await this.$axios.post(url, producto);
      console.log(data);

      this.getProductos();

      this.modeloAgregar = true;
      this.idProductos = "";
      this.nombre = "";
      this.precio = "";
      this.imagen = null;
      this.descripcion = "";
      }else{
        this.modeloAlerta = true;
      }
    },

    async eliminar(id_producto, index){
      
      let url = url_api + 'productos/' + id_producto;
      let  {data} = await this.$axios.delete(url);
      this.productos.splice(index,1);
      this.modeloEliminado = true;
      this.modeloEliminar = false;
    },

    async editar(indexProd){
      console.log(indexProd);
      
      let url = url_api + 'productos';
      let {data} = await this.$axios.get(url, {id_producto: indexProd});
      console.log(data);
    },
    
    async actualizar(indexProd){

      let url = url_api + 'productos';
      let {data} = await this.$axios.put(url, {id_producto: indexProd});
      console.log(data.info);
      //this.productos = data.info;

      this.nombre = data.info[0].nombre ;
      this.precio = data.info[0].precio_base;
      this.imagen= "";
      
      this.descripcion= data.info[0].descripcion;

    },

  },
}


</script>