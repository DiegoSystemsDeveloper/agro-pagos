<template>
    <div>
        <div>
            <h2 class="text-center">Directorio transportistas</h2>
        </div>
        <div class="m-5">
            <b-table striped hover :items="transportistas" :fields="fields">
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
            transportistas: [],
            fields: ['nombre', 'apellido', 'telefono', 'correo'],
        }
            
    },

    mounted: async function(){
        let usuario = JSON.parse(localStorage.getItem('userIn'))
        this.id_usuario = usuario.id_usuario;
        let url = url_api + 'usuarioTransporte';

        let {data} = await this.$axios.get(url);
        console.log(data.info);

        this.transportistas = data.info;

    },

}
</script>