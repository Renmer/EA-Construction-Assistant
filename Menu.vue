<template>
<div class="menu">
  <b-container fluid>
    <h3 align="center">Menu</h3>
    <div v-for="tarjeta in tarjeta" :key="tarjeta.proyecto" align="center">
      <b-card align="left" style="max-width: 800px;">
        <b-img-lazy id="image_proyecto" :src="tarjeta.img" alt="Card image" style=" height: 250px; width: 300px;" fluid right></b-img-lazy>
        <b-card-title left>Proyecto: {{tarjeta.proyecto}} </b-card-title>
        <b-card-sub-title>Direccion: {{tarjeta.direccion}}</b-card-sub-title>
        <b-card-text> <b>Descripcion:</b> {{tarjeta.descripcion}} </b-card-text>
        <b-card-text>
          <b>Tiempo restante:</b> {{tarjeta.dias}}
          <br>
          <b>Progreso:</b>
          <b-progress style="max-width: 400px;" :value="tarjeta.progreso" :max="max" show-progress animated></b-progress>
        </b-card-text>
        <b-button-group>
          <b-button pill :to="{name:'infoproyecto', params:{data: tarjeta.proyecto}}" class="card-link">Ver proyecto</b-button>
          <b-button pill :to="{name:'generadores', params:{data: tarjeta.proyecto}}" class="card-link">Ver conceptos</b-button>
          <b-button pill :to="{name:'planos', params:{data: tarjeta.proyecto}}" class="card-link">Ver Planos</b-button>
        </b-button-group>
      </b-card>
      <br>
    </div>
    <div>
      <b-button class="float" to="/creacion">Creacion</b-button>
    </div>
  </b-container>
</div>
</template>

<script>
import {db} from '../firebaseConfig'//importacion de la base de datos al componente
import firebase from "firebase/app"
import 'firebase/storage'
export default {
    data() {
      return {
        tarjeta:[],
        max: 100,
        diasrestantes: null,
      }
    },
    created(){
      
      this.carga_datos().then( ()=>{
        const perfil = firebase.storage().ref();
        this.tarjeta.forEach((doc) =>{
            perfil.child(`Proyectos/${doc.proyecto}/ImagenProyecto`).listAll().then(function(image){
              if(image._delegate.items.length > 0){
                image.items.forEach(function(itemref){
                itemref.getDownloadURL().then(function (url){
                  if(url == ''){
                    console.log('es nulo');
                  }
                    doc.img = url;
                  })
                });
              }
              else{
                doc.img = 'https://placekitten.com/300/300';
              }
              
            }).catch((error) =>{console.log(error)});
         })
      })
    },
    mounted(){
      
    },
    methods:{
      carga_datos(){
        return Promise.resolve(
        db.collection("Proyectos").get().then((querySnapshot) => {
          querySnapshot.forEach((doc) => {
            const datos ={
              proyecto: doc.id,
              direccion: doc.data().direccion,
              descripcion: doc.data().descripcion,
              dias: this.calculo(doc.data().inicio,doc.data().fin),
              progreso: doc.data().progreso,
              img: null,
            }
              this.tarjeta.push(datos);
            });
          })
        );
      },
      calculo (fechainicio, fechafin){//funcion para calcular el tiempo restante para concluir el proyecto
        const ini = new Date(fechainicio);
        const fin = new Date(fechafin);
        var restante = Math.abs(fin-ini);
        var diascompletos = restante/(1000*3600*24);
        let today = new Date().toISOString().slice(0, 10)
        const hoy = new Date(today);
        var consumidos = Math.abs(hoy-ini);
        var diasconsumidos = consumidos/(1000*3600*24);
        var restantes = diascompletos-diasconsumidos;
        if(restantes < 0){
          if(fin < hoy){
            return 'El proyecto ha finalizado'
          }
          else{
            var message = "Faltan "+Math.abs(restantes)+" dias para que comience el proyecto";
            return message;
          }
          
        }
        else{
          return restantes +" dias";
        }
      }
    },
    
    
  }
</script>

<style>
@media only screen and (max-width: 414px) {

}
@media only screen and (max-width: 768px) {

}
@media only screen and (max-width: 1920px) {
  body{
    font-family: Arial, Helvetica, sans-serif;
  }
  h1{
    margin: 1%;
  }
  .float{
	position:fixed;
	width:auto;
	height:auto;
	bottom:40px;
	right:40px;
	
	color:#FFF;
	text-align:center;

  }
}

</style>
