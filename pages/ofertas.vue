<template>
    <div>
        <h1 class="text-center">CRUD OFERTAS</h1>

        <b-container v-show="true">
            <label>Seleccione un producto</label>
            <b-form-select v-model="selected" :options="options" required>
                   
            </b-form-select>
            <p>Valor producto: {{selected}}</p>
            
            <b-form-group label="Seleccione modo de oferta" class="my-2">
            <b-form-radio-group
                v-click = 'cambiarFormulario()'
                v-model="pago"
                :options="pagos"
                name="radio-inline"
            ></b-form-radio-group>
            </b-form-group>
        </b-container>

        <b-container v-show="monetario">
            <h4>Oferta monetaria</h4>
            <label>Ingrese valor a ofertar</label>
            <b-form-input type="number" v-model="valorOferta" required></b-form-input>

        </b-container>

        <b-container v-show="trueque">
            <h4>Oferta de trueque</h4>
            <label >Inserte una imagen del elemento a intercambiar</label>
            <b-form-file v-model="valorOferta" class="mt-3" plain required></b-form-file>
            <div class="mt-3">Nombre imagen: {{ valorOferta ? valorOferta.name : '' }}</div>

        </b-container>

        <b-container class="my-5">
            <b-button @click="agregar" variant="success">Crear</b-button>
            <b-button @click="actualizar" variant="primary" v-show="estadoBtn">Actualizar</b-button>
        </b-container>

        <div>
            <b-table striped hover :items="ofertas" :fields="fields">
                <template v-slot:cell(acciones)="row">
                    <b-button variant="danger" size="sm" @click="eliminar(row.index)" class="mr-1">
                    Eliminar
                    </b-button>
                    <b-button variant="primary" size="sm" @click="editar(row.index)" class="mr-1">
                    Editar
                    </b-button>
                    
                </template>
            </b-table>
        </div>

    </div>

</template>

<script>
export default {

    name: 'Form',
    
    data(){
        return{
            
            selected: null,
            pago: null,
            monetario: false,
            trueque: false,

            producto: null,
            precio: null,

            prueba: null,
            valorOferta: null,

            file2: null,

            iGlobal: null,
            estadoBtn: false,

            ofertas: [],
            fields: ['producto',  'valor', 'metodo', "oferta", "acciones"],

            pagos:[
                {text: 'Oferta monetaria', value: 'Monetario'},
                {text: 'Trueque', value: 'Trueque', }
            ],

            options: [
                { value: null, text: 'Productos', disabled: true },
                { value: '100.000', text: 'Aguacate hass x10Kilos' },
                { value: '240.000', text: 'Lote de manzana verde x80Kilos' },
                { value: '180.000', text: 'Limon tahiti x8Kilos' },
                { value: '200.000', text: 'Piña oro miel x50Kilos' },
                { value: '120.000', text: 'Tomate cherry x20Kilos'}
            ],
 
        }
        
            
    },

    methods:{

        cambiarFormulario(){
            if(this.pago === 'Monetario'){
               this.monetario = true;
               this.trueque = false;
            }
            if(this.pago === 'Trueque'){
                this.monetario = false;
                this.trueque = true;
            }
        },

        buscar(){
            this.prueba = this.options.find(valor => valor.value === this.selected);
            this.precio = this.prueba.value;
            this.producto = this.prueba.text;
        },

        agregar(){

            this.buscar();
            this.ofertas.push({
                producto: this.producto,
                valor: this.precio,
                metodo: this.pago,
                oferta: this.valorOferta
            });
            localStorage.setItem('Ofetas', JSON.stringify(this.ofertas));
            console.log(this.ofertas);
            
            this.selected = null;
            this.pago = null;
            this.valorOferta = null;
            this.monetario= false;
            this.trueque= false;

        },

        eliminar(i){
            this.ofertas.splice(i,1);
            localStorage.setItem('Ofetas', JSON.stringify(this.ofertas));
        },

        editar(i){

            this.iGlobal = i;
            this.estadoBtn = true;

            if(this.ofertas[i].metodo === 'Monetario'){

                this.options[i].text = this.ofertas[i].producto;
                this.selected = this.ofertas[i].valor;
                this.pago = this.ofertas[i].metodo;
                this.valorOferta = this.ofertas[i].oferta;
                this.monetario = true;
                console.log(this.iGlobal);
            }

            if(this.ofertas[i].metodo === 'Trueque'){

                this.options[i].text = this.ofertas[i].producto;
                this.selected = this.ofertas[i].valor;
                this.pago = this.ofertas[i].metodo;
                this.valorOferta = this.ofertas[i].oferta;
                this.trueque = true;
                console.log(this.iGlobal);
            }
            
        },

        actualizar(){
            this.buscar();
            this.ofertas[this.iGlobal].producto = this.producto;
            this.ofertas[this.iGlobal].valor = this.precio;
            this.ofertas[this.iGlobal].metodo = this.pago;
            this.ofertas[this.iGlobal].oferta = this.valorOferta;

            localStorage.setItem('Ofetas', JSON.stringify(this.ofertas));

            this.estadoBtn = false;
            this.selected = null;
            this.pago = null;
            this.valorOferta = null;
            this.monetario= false;
            this.trueque= false;
        }
    },   
}
</script>