<template>
    <div >
        <div>
            <h2 class="text-center">Ofertas</h2>
        </div>
        <div class="m-5">
            <b-table striped hover :items="ofertas" :fields="fields">
            <template v-slot:cell(acciones)="row">
                <b-button variant="success" size="sm" @click="cambiarEstado(ofertas[row.index].id_oferta, row.index)" class="mr-1">
                Aceptar
                </b-button>
                <b-button variant="danger" size="sm" @click="solicitudEliminar(row.index)" class="mr-1">
                Eliminar
                </b-button>
            </template>
        </b-table>
        </div>
        <div class="mt-2">
            <h2 class="text-center">Ofertas aceptadas</h2>
        </div>
        <div class="m-5">
            <b-table striped hover :items="ofertasAceptadas" :fields="fields2">
        </b-table>
        </div>

    </div>
</template>

<script>
import { isNull } from 'util';
import config from "../assets/config";
const url_api = config.API_URL;
export default {
    name: 'Ofertas',
    data(){
        return{
            modeloEliminar: null,
            modeloEliminado: null,
            
            filaEliminar: null,

            ofertas: [],
            ofertasAceptadas: [],
            fields: ['usuario','producto', 'precio', 'metodos', 'valoroferta', "acciones"],

            fields2: ['usuario','producto', 'precio', 'metodos', 'valoroferta', "estado"],
        }
            
    },

    mounted: async function(){
        let usuario = JSON.parse(localStorage.getItem('userIn'))
        this.id_usuario = usuario.id_usuario;
        let url = url_api + 'ofertas/' + usuario.id_usuario;
        let {data} = await this.$axios.get(url);
        this.ofertas = data.info;
        
        let url2 = url_api + 'ofertaAgri/' + usuario.id_usuario;
        let res = await this.$axios.get(url2);
        
        this.ofertasAceptadas = res.data.info;
        console.log(this.ofertasAceptadas);

    },

    methods: {

        async cambiarEstado(id, index){
            
            let url = url_api + 'ofertas';
            let parametros={
                id_oferta: id,
                estado: "aceptado"
            }
            let {data} = await this.$axios.put(url, parametros);
            this.ofertasAceptadas.push(this.ofertas[index]);
            this.ofertas.splice(index,1);
            
            console.log(data);
           
        }
    }
}
</script>