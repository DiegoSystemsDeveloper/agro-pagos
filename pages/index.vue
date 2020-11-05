<template>
  <div>
    <Navegacion />
    <div id="fondo">
      <h1>AGRO PAGOS</h1>
      <a v-b-modal.modal-1>REGISTRARME</a>
      <p>Que vendes?<br>Que compras?<br>Somos una red para negociar con proveedores.</p>
    </div>
  </div>
</template>

<script>
import Login from "./login";
import Navegacion from "../components/Navegacion";
import Agricultor from "../components/FormularioAgricultor"
export default {
  components: {
    Login,
    Navegacion,
    Agricultor
  }
};
</script>
<style>

body{
  background-color:rgb(219, 217, 217);
  border-radius: 10px;
}

#fondo{
  background-image: url('../static/images/logo3.jpg');
  background-position: center;
  background-size: cover;
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  box-shadow: 0px 5px 7px rgba(0, 0, 0, 0.521);
  text-justify: initial;
  text-align: center;
  align-items: center;
  margin: 5px 10px;
  border-radius: 5px;
  height: 93vh;
}

#fondo h1{
  padding: 20px;
  font-size: 55px;
  font-family: 'Times New Roman';
  text-shadow: 0px 2px 3px rgba(0, 0, 0, 0.801);
  color: rgb(255, 255, 255);
}

#fondo p{
  color: #fff;
  font-size: 16px;
  text-shadow: 0px 3px 4px black;
}

#fondo a{
  text-decoration: none;
  color: white;
  font-size: 21px;
  background: rgb(13, 194, 52);
  border-radius: 20px;
  padding: 2vh 5vh 2vh 5vh;
  transition: 0.1s;
}

#fondo a:hover{
  background-color: rgb(33, 175, 52);
  transform: scale(1.1,1.1);
}

</style>