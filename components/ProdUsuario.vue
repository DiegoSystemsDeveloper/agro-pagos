<template>
    <div class="mt-2 ml-2">
        
        <h3 class="text-center">PRODUCTOS</h3>

        <div>
            <b-carousel
              id="carousel-fade"
              style="text-shadow: 0px 0px 2px #000"
              fade
              indicators
              img-width="1024"
              img-height="480"
            >
              <b-carousel-slide
                caption="Bienvenidos a agro pagos"
                img-src="https://picsum.photos/1000/350/?image=517"
              ></b-carousel-slide>

              <b-carousel-slide
                caption="Elimine intermediarios en su compra"
                img-src="https://picsum.photos/1000/350/?image=1080"
              ></b-carousel-slide>
            </b-carousel>
            
            <hr noshade="noshade">
        </div>

        
        
        <b-card-group columns class="mt-3">
            <b-card v-for="(producto, index) in productos" :key="index"
            tag="article"
            style="max-width: 24rem;"
            class="ml-2"
            >
              <h5 class="text-center">Producto: {{producto.nombre}}</h5>
              <b-img center class="mt-3"  width=300px height=250px  src="https://www.lavozdelnorte.cl/wp-content/uploads/2019/12/frutas-verduras-730x482.jpg" alt="Center image"></b-img>
              <p class="mt-2">Precio base: {{producto.precio_base}} </p>
              <p>Imagen:</p>
              <p>Descripcion: {{producto.descripcion}}</p>
              <b-button variant="success" @click="mostrarOcultar(index)" v-show="btnOfertas" size="sm" block pill>
                Ofertar
              </b-button>

            <b-form v-show="producto.panel">
                
                <b-form-group label = "Metodos de oferta" class = "my-2" >
                    <b-form-radio-group
                        v-model="modo"
                        v-click='cambiarFormulario()'
                        :options="pagos"
                        name="radio-validation"
                        :state="state"
                    >
                    <b-form-invalid-feedback :state="state">Elige uno por favor.</b-form-invalid-feedback>
                    <b-form-valid-feedback :state="state">Gracias</b-form-valid-feedback>
                    </b-form-radio-group>
                </b-form-group>

                <b-container v-show="monetario">

                    <div role="group">
                        <b-form-input
                            type="number"
                            size="sm"
                            id="input-live"
                            v-model="ofertaPesos"
                            :state="ofertaState"
                            aria-describedby="input-live-help input-live-feedback"
                            placeholder="Ingrese la oferta"
                            trim
                        ></b-form-input>
                        <b-form-invalid-feedback id="input-live-feedback">
                          Ingresa el valor en miles
                        </b-form-invalid-feedback>
                        <b-form-text id="input-live-help">Tu oferta esta completa</b-form-text>
                    </div>
                </b-container>

                <b-container v-show="trueque">
                    <h4>Oferta de trueque</h4>
                    <label >Inserte una imagen del elemento a intercambiar</label>
                    <b-form-file
                        size="sm"
                        v-model="file1"
                        :state="Boolean(file1)"
                        placeholder="Choose a file or drop it here..."
                        drop-placeholder="Drop file here..."
                    ></b-form-file>
                    <div class="mt-3">Selected file: {{ file1 ? file1.name : '' }}</div>

                </b-container>

                <b-container class="my-5">
                    <b-button variant="success" @click="agregarOferta(producto.id_producto)" size="sm" block pill>Enviar oferta</b-button>
                    <b-button variant="danger" @click="cancelarOferta" size="sm" block pill>Cancelar oferta</b-button>
                </b-container>
            </b-form>
             
        </b-card>

        </b-card-group>

        <b-container>
            <b-modal 
                v-model="modalIncompleto" 
                size="sm" 
                title="Aviso"
                ok-only
            ><p class="my-1">Debes completar todos los campos</p>
            </b-modal>
            <b-modal 
                v-model="modalAgregado" 
                size="sm" 
                title="Aviso"
                ok-only
            ><p class="my-1">Tu oferta ha sido enviada</p>
            </b-modal>
        </b-container>
    </div>
    
</template>

<script>
import config from "../assets/config";
const url_api = config.API_URL;
import { isNull, log } from 'util';
export default {

    name: 'ProductoAgricultor',
    data(){
        return{
            id_usuario: null,

            modalIncompleto: false,
            modalAgregado: false,
            
            productos:[],
            ofertas:[],

            modo: false,
            ofertaPesos: "",
            file1: null,
            btnOfertas: true,

            monetario: false,
            trueque: false,

            anterior: null,
            anterioAux: null,

            pagos:[
                {text: 'Oferta monetaria', value: 'Monetario'},
                {text: 'Trueque', value: 'Trueque', }
            ],
            
        }
    },

    mounted: async function(){
        let usuario = JSON.parse(localStorage.getItem('userIn'))
        this.id_usuario = usuario.id_usuario;
        let url = url_api + 'productos';
        let {data} = await this.$axios.get(url);
        this.productos = data.info;
        console.log(this.id_usuario);
        console.log(data.info);
    },

    methods: {

        async agregarOferta(idProducto){
            
            let url = url_api + 'ofertas';
            let parametros = {};

            if(this.modo === 'Monetario' && this.ofertaPesos != ''){
                let url = url_api + 'ofertas';
                
                parametros.id_producto = idProducto;
                parametros.metodos = this.modo;
                parametros.ValorOferta = this.ofertaPesos;
                parametros.cliente = this.id_usuario;
                parametros.estado = "pendiente";
               
                let  {data} = await this.$axios.post(url, parametros);
                this.productos[this.anterior].panel = false;
                this.productos[this.anterioAux].panel = false;
                this.modalAgregado = true;

            }else if(this.modo === 'Trueque' && this.file1 != null){
                
                parametros.id_producto = idProducto;
                parametros.metodos = this.modo;
                parametros.ValorOferta = this.file1.name;
                parametros.cliente = this.id_usuario;
                parametros.estado = "pendiente"

                let  {data} = await this.$axios.post(url, parametros);
                this.productos[this.anterior].panel = false;
                this.productos[this.anterioAux].panel = false;
                this.modalAgregado = true;
            }else{
                this.modalIncompleto = true;
            }
            this.btnOfertas = true;
        },

        cancelarOferta(){
            this.btnOfertas = true;
            this.productos[this.anterior].panel = false;
            this.productos[this.anterioAux].panel = false;
        },

        mostrarOcultar(actual){
            this.todoFalso();
            this.btnOfertas = false;

            if(isNull(this.anterior)){
                this.productos[actual].panel = true;
                this.anterior = actual;
                this.anterioAux = this.anterior;

            }else{

                this.productos[this.anterior].panel = false;
                this.anterioAux = this.anterior;
                this.anterior = actual;
                this.productos[actual].panel = true;

            }
        },

        todoFalso(){
            this.modo = false;
            this.monetario = false;
            this.trueque = false;
            this.ofertaPesos = "";
            this.file1 = null;
        },

        cambiarFormulario(){
            
            if(this.modo === 'Monetario'){
               this.monetario = true;
               this.trueque = false;
            }
            if(this.modo === 'Trueque'){
                this.monetario = false;
                this.trueque = true;
            }
        },
        
    },

    computed: {

      state() {
        return Boolean(this.modo)
      },

      ofertaState() {
        return this.ofertaPesos.length > 3 ? true : false
      }

    }, 

}
</script>