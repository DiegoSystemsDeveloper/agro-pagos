<template>
    <div>
        <h1 class="text-center">CRUD VIDEOS</h1>

        <b-container>

            <b-form >
                <b-avatar class="mr-3" src="https://www.elheraldo.co/sites/default/files/articulo/2018/03/30/1b_campo.jpg" size="6rem"></b-avatar>
                <span class="mr-auto">Jose Alonso Perdomo</span>
                <br>

                
                <b-container class="mt-3" v-show="formVideo">
                    <label>Sube tu video</label>
                    <b-form-file
                            v-model="fileVideo"
                            :state="Boolean(fileVideo)"
                            placeholder="Busca el video que que desear agregar"
                            drop-placeholder="Drop file here..."
                            accept=".mp4"
                    ></b-form-file>
                    <div class="mt-3">Video seleccionado: {{ fileVideo ? fileVideo.name : '' }}</div>

                    <b-button variant="success" v-show="!btnActualizar" size="sm" @click="agregar()" class="mr-1">
                        Guardar
                    </b-button>
                    <b-button variant="primary" v-show="btnActualizar" size="sm" @click="actualizar()" class="mr-1">
                        Actualizar
                    </b-button>

                </b-container>
                
                <h4 class="mt-3">Mis productos</h4>
                <div class="mt-3">
                    <b-table striped hover :items="productos" :fields="fields">
                        <template v-slot:cell(acciones)="row">
                            <b-button variant="success" size="sm" @click="abrirFormulario(row.index)" class="mr-1">
                            Agregar video
                            </b-button>
                        </template>
                    </b-table>
                </div>

                <div>
                    <h4 class="mt-5">Mis productos con videos</h4>
                    <b-table class="mt-3" striped hover :items="productosVideos" :fields="fieldsVideos">
                        <template v-slot:cell(acciones)="row">
                            
                            <b-button variant="danger" size="sm" @click="solicitudEliminar(row.index)" class="mr-1">
                            Eliminar video
                            </b-button>
                            <b-button variant="primary" size="sm" @click="editar(row.index)" class="mr-1">
                            Editar video
                            </b-button>

                        </template>
                    </b-table>
                </div>

            </b-form>

            <b-modal
                v-model="modeloAgregar" 
                size="sm" 
                title="Mensaje" 
                ok-only
            ><p class="my-1">Tu video ha sido subido</p>
            </b-modal>
            
            <b-modal id="my-modal" size="sm" title="Eliminar" v-model="modeloEliminar" hide-footer>
                    <div class="d-block">¿Desea eliminar el video subido?</div>
                    <br>
                    <b-button @click="eliminar" variant="success">Si</b-button>
                    <b-button @click="modeloEliminar = false"  variant="danger" >No</b-button>
            </b-modal>

            <b-modal 
                v-model="modeloAlerta" 
                size="sm" 
                title="Aviso"
                ok-only
            ><p class="my-1">Debes completar los campos en rojo</p>
            </b-modal>

            <b-modal 
                v-model="modeloActualizado" 
                size="sm" 
                title="Actualizado" 
                ok-only
            ><p class="my-1">Oferta actualizada</p>
            </b-modal>
            
            <b-modal 
                v-model="modeloEliminado" 
                size="sm" 
                title="Eliminado" 
                ok-only
            ><p class="my-1">Oferta eliminada</p>
            </b-modal>

        </b-container>

    </div>

</template>

<script>

export default {
    name: 'Videos',
    data(){
        return{
            selected: null,
            fileVideo: null,
            formVideo: false,
            rowGlobal: 0,
            btnActualizar: false,

            modeloAlerta: null,
            modeloAgregar: null,
            modeloEliminar: null,
            modeloEliminado: null,
            modeloActualizado: null,

            options: [
                { value: null, text: 'Productos', disabled: true },
                { value: '100.000', text: 'Aguacate hass x10Kilos'},
                { value: '240.000', text: 'Lote de manzana verde x80Kilos' },
                { value: '180.000', text: 'Limon tahiti x8Kilos' },
                { value: '200.000', text: 'Piña oro miel x50Kilos' },
                { value: '120.000', text: 'Tomate cherry x20Kilos'}
            ],

            fields:['id', 'nombre', 'precio', 'acciones'],
            fieldsVideos:['id', 'nombre', 'precio', 'video', 'acciones'],

            productos:[
                {id:'1',nombre: 'Mango tomy x10Kilos', precio:'120.000'},
                {id:'2',nombre: 'Manzana roja x20Kilos', precio:'90.000'},
                {id:'3',nombre: 'Piña oro miel x15Kilos', precio:'180.000'}
            ],

            productosVideos:[]
        }
    },

    mounted: function(){

        let datos = JSON.parse(localStorage.getItem('ProdVideos'))
        if(datos === null){
            this.productosVideos = [];
        }else{
            this.productosVideos = datos;  
            
            for(let i = 0; i<this.productos.length ; i++){
                if(this.productosVideos.find(busqueda => busqueda.id === this.productos[i].id)){
                    this.productos.splice(i,1);
                }
            }    
        }
    },

    methods:{

        abrirFormulario(i){
            this.btnActualizar = false;
            this.formVideo = true;
            this.rowGlobal = i;
        },

       
        agregar(){

            if(this.fileVideo != null){
                this.productosVideos.push({
                    id: this.productos[this.rowGlobal].id,
                    nombre: this.productos[this.rowGlobal].nombre,
                    precio: this.productos[this.rowGlobal].precio,
                    video: this.fileVideo.name,
                    file: this.fileVideo
                });
                localStorage.setItem('ProdVideos', JSON.stringify(this.productosVideos));
                this.modeloAgregar = true;
                this.productos.splice(this.rowGlobal,1);
                this.fileVideo = null;
                this.formVideo = false;
                this.rowGlobal = 0;
            }else{
                this.modeloAlerta = true;

            }

        },

        solicitudEliminar(i){
            this.rowGlobal = i;
            this.modeloEliminar = true;
        },

        eliminar(){
            this.productos.push({
                id: this.productosVideos[this.rowGlobal].id,
                nombre: this.productosVideos[this.rowGlobal].nombre,
                precio: this.productosVideos[this.rowGlobal].precio,
            });
            this.productosVideos.splice(this.rowGlobal,1);

            localStorage.setItem('ProdVideos', JSON.stringify(this.productosVideos));

            this.modeloEliminar = false;
            this.modeloEliminado = true;
        },

        editar(i){
            this.rowGlobal = i;
            this.fileVideo = this.productosVideos[this.rowGlobal].file; 
            this.formVideo = true;
            this.btnActualizar = true;
        },

        actualizar(){
            this.productosVideos[this.rowGlobal].video = this.fileVideo.name;
            this.productosVideos[this.rowGlobal].file = this.fileVideo;
            localStorage.setItem('ProdVideos', JSON.stringify(this.productosVideos));
            this.modeloActualizado = true;
            this.formVideo = false;
            this.fileVideo = null;
            this.btnActualizar = false;
        }
    },

}
</script>