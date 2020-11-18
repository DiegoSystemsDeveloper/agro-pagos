<template>
    <div class="mt-2 ml-2">
        <b-card-group columns>
       
        <b-card v-for="(producto, index) in productos" :key="index"
            title=""
            tag="article"
            style="max-width: 20rem;"
            class="mb-2"
            >
              <h5 class="text-center">Producto: {{producto.nombre}}</h5>
              <p>Precio base: {{producto.precio}} </p>
              <p>Imagen: {{producto.imagen.name}}</p>
              <p>Descripcion: {{producto.descripcion}}</p>
              <b-button variant="success" @click="mostrarOcultar(index)" v-show="btnOfertas" class="mr-1" size="sm" block pill>
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
                    <b-button variant="success" @click="agregarOferta(producto.id)" size="sm" block pill>Enviar</b-button>
                    <b-button variant="danger" @click="cancelarOferta" size="sm" block pill>Cancelar</b-button>
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
        </b-container>
    </div>
    
</template>

<script>
import { isNull, log } from 'util';
export default {

    name: 'ProductoAgricultor',
    data(){
        return{
            modalIncompleto: false,
            
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

    mounted: function(){

        let datos = JSON.parse(localStorage.getItem('ProductosAgricultor'))
        if(datos === null){
            this.productos = [];
        }else{
            this.productos = datos; 
        }

    },

    methods: {

        agregarOferta(idProducto){

            if(this.modo === 'Monetario' && this.ofertaPesos != ''){
                this.ofertas.push({
                    id: 1,
                    idProducto: idProducto,
                    metodo: this.modo,
                    oferta: this.ofertaPesos
                });

                localStorage.setItem('Ofertas', JSON.stringify(this.ofertas));
                console.log(this.ofertas);
                this.productos[this.anterior].panel = false;
                this.productos[this.anterioAux].panel = false;

            }else if(this.modo === 'Trueque' && this.file1 != null){
                
                this.ofertas.push({
                    id: 1,
                    idProducto: idProducto,
                    metodo: this.modo,
                    oferta: this.file1.name
                });
                localStorage.setItem('Ofertas', JSON.stringify(this.ofertas));
                console.log(this.ofertas);
                this.btnOfertas = true;
                this.productos[this.anterior].panel = false;
                this.productos[this.anterioAux].panel = false;
            }else{
                this.modalIncompleto = true;
            }

            
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