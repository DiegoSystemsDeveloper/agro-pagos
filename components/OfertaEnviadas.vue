<template>
    <div >
        <div>
            <h2 class="text-center">Ofertas enviadas</h2>
        </div>
        <div class="m-5">
            <b-table striped hover :items="ofertas" :fields="fields">
            <template v-slot:cell(acciones)="row">
                <b-button variant="success" size="sm" @click="cambiarEstado(ofertas[row.index].id_oferta)" class="mr-1">
                Aceptar
                </b-button>
                <b-button variant="danger" size="sm" @click="solicitudEliminar(row.index)" class="mr-1">
                Eliminar
                </b-button>
            </template>
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
            fields: ['usuario','producto', 'telefono', 'precio', 'metodos', 'valoroferta', "estado"],
        }
            
    },

    mounted: async function(){
        let usuario = JSON.parse(localStorage.getItem('userIn'))
        this.id_usuario = usuario.id_usuario;
        let url = url_api + 'otros/' + usuario.id_usuario;

        let {data} = await this.$axios.get(url);
        console.log(data.info);

        this.ofertas = data.info;

    },

    methods: {

        async cambiarEstado(id){
            
            let url = url_api + 'ofertas';
            let parametros={
                id_oferta: id,
                estado: "aceptado"
            }
            let {data} = await this.$axios.put(url, parametros);
            console.log(data);
           
        }
    }
}
</script>