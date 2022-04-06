<template>
  <div class="creacion">
    <!-- Definicion del proyecto -->
    <div id="definicion" v-show="visible == 1">
      <b-container fluid>
        <h3 align="center">Detalles generales del proyecto</h3>
        <b-row>
          <b-col> <label >Nombre del proyecto</label> <b-input id="proyecto" v-model="proyecto"  placeholder="" required></b-input></b-col>
          <b-col> <label >Propietario</label><b-input id="propietario" v-model="propietario" placeholder=""></b-input> </b-col>
        </b-row>
        <b-row>
          <b-col> <label >Descripcion del proyecto</label> <b-textarea id="descripcion" v-model="descripcion" placeholder=""></b-textarea></b-col>
          <b-col>
            <b-row>
              <b-col> <label >Fecha de inicio</label> <b-form-datepicker id="fecha_inicio" v-model="fecha_inicio" placeholder=""></b-form-datepicker></b-col>
              <b-col> <label >Fecha de finalización</label> <b-form-datepicker id="fecha_fin" v-model="fecha_fin" placeholder=""></b-form-datepicker></b-col>
            </b-row>
          </b-col>
        </b-row>
        <b-row>
          <b-col> <label >Direccion de la obra</label> <b-input id="direccion" v-model="direccion" placeholder=""></b-input></b-col>
          <b-col><label >Contrato</label> <b-input id="contrato" v-model="contrato" placeholder=""></b-input></b-col>
          <b-col><label >Encargado de obra</label> <b-input id="encargado" v-model="encargado" placeholder=""></b-input></b-col>
        </b-row>
        <b-row> 
          <b-col><label >Construccion (mts2)</label><b-input id="construccion" v-model="construccion" placeholder=""></b-input></b-col>
          <b-col><label >Remodelacion (mts2)</label><b-input id="remodelacion" v-model="remodelacion" placeholder=""></b-input> </b-col>
          <b-col>
            <label >Portada del proyecto</label>
              <b-input-group class="mb-3">
                <b-input-group-prepend is-text> <b-form-checkbox v-model="select" class="mr-n2"/> </b-input-group-prepend>
                <b-form-file v-model="imagen" multiple :disabled="!select"></b-form-file>
              </b-input-group>
          </b-col>
        </b-row>
      </b-container>
    </div>
    <!-- seleccion de modulos -->
    
    <div id="seleccion" v-show="visible == 2">
      <b-container fluid>
      <h3 align="center">Catalogo de conceptos</h3>
      <b-row>
        <b-col> <Desmontajes :nombre_proyecto="proyecto"/> </b-col>
      </b-row>
      <b-row>
        <b-col> <Preliminares :nombre_proyecto="proyecto"/> </b-col>
      </b-row>
      <b-row>
        <b-col> <Terracerias :nombre_proyecto="proyecto"/> </b-col>
      </b-row>
      <b-row>
        <b-col> <Cimentacion :nombre_proyecto="proyecto"/> </b-col>
      </b-row>
      <b-row>
        <b-col> <Estructura :nombre_proyecto="proyecto"/></b-col>
      </b-row>
      <b-row>
        <b-col> <Albanileria :nombre_proyecto="proyecto"/>  </b-col>
      </b-row>
      <b-row>
        <b-col> <Limpieza :nombre_proyecto="proyecto"/>  </b-col>
      </b-row>
      <b-row>
        <b-col><Acabados :nombre_proyecto="proyecto"/></b-col>
      </b-row>
      <b-row>
        <b-col><Hac :nombre_proyecto="proyecto"/></b-col>
      </b-row>
      <b-row>
        <b-col><Carpinteria :nombre_proyecto="proyecto"/></b-col>
      </b-row>
      <b-row>
        <b-col><Hidrosanitaria :nombre_proyecto="proyecto"/></b-col>
      </b-row>
      <b-row>
        <b-col><Electrico :nombre_proyecto="proyecto"/></b-col>
      </b-row>
      <b-row>
        <b-col><Circuito :nombre_proyecto="proyecto"/></b-col>
      </b-row>
      <b-row>
        <b-col><Iluminacion :nombre_proyecto="proyecto"/></b-col>
      </b-row>
      </b-container>
    </div>
    
    <!-- ajustes finales -->
    <div id="finalizacion" v-show="visible == 3">
      <Planos :nombre_proyecto="proyecto"></Planos>
    </div>
    <b-container fluid>
      <b-row>
        <b-col>
          <b-button-group>
            <b-button id="atras" v-show="visible > 1" @click="atr">Atras</b-button>
            <b-button id="siguiente" v-show="visible != 3" @click="sig">Siguiente</b-button>
            <b-button id="fin" v-show="visible == 3" @click="finalizar">Finalizar</b-button>
          </b-button-group>
        </b-col>
      </b-row>
    </b-container>
    
    
  </div>
</template>

<script>
import {db} from '../firebaseConfig'//importacion de la base de datos al componente
import firebase from "firebase/app"
import 'firebase/storage'
import Planos from '../modulos/carga_planos.vue'
import Desmontajes from '../modulos/desmontajes.vue'
import Preliminares from '../modulos/preliminares.vue'
import Terracerias from '../modulos/terracerias.vue'
import Cimentacion from '../modulos/cimentacion.vue'
import Estructura from '../modulos/estructura.vue'
import Albanileria from '../modulos/albanileria.vue'
import Acabados from '../modulos/acabados.vue'
import Hac from '../modulos/hac.vue'
import Carpinteria from '../modulos/carpinteria.vue'
import Hidrosanitaria from '../modulos/hidrosanitaria.vue'
import Electrico from '../modulos/electrico.vue'
import Circuito from '../modulos/circuitocerrado.vue'
import Iluminacion from '../modulos/iluminacion.vue'
import Limpieza from '../modulos/limpieza.vue'

export default {
  components: { 
    Desmontajes,
    Planos,
    Preliminares,
    Terracerias,
    Cimentacion,
    Estructura,
    Albanileria,
    Acabados,
    Hac,
    Carpinteria,
    Hidrosanitaria,
    Electrico,
    Circuito,
    Iluminacion,
    Limpieza
  },
  data() {
    return{
      proyecto: null,
      descripcion: null,
      fecha_inicio: null,
      fecha_fin: null,
      direccion: null,
      propietario: null,
      contrato: null,
      construccion: null,
      remodelacion: null,
      encargado: null,
      visible: 1,
      value:null,
      imagen: null,
      select: null,
      
    }
  },
  methods:{
    sig(){
      if (this.visible > 0 || this.visible < 4) {
        this.visible += 1
      }
    },
    atr(){
      if(this.visible > 1){
          this.visible -= 1
        }
      
    },
    finalizar(){
      if(this.imagen != null && this.select == true){
        firebase.storage().ref(`Proyectos/${this.nombre_proyecto}/ImagenProyecto/${this.imagen.name}`).put(this.imagen);
      }
      else{
        alert("Ingresa una imagen");
        return
      }
      db.collection('Proyectos').doc(this.proyecto).set({
        descripcion: this.descripcion,
        inicio: this.fecha_inicio,
        fin: this.fecha_fin,
        direccion: this.direccion,
        propietario: this.propietario,
        contrato: this.contrato,
        construccion: this.construccion,
        remodelacion: this.remodelacion,
        encargado: this.encargado,
        progreso: 0
        });
        this.$root.$refs.Desmontajes.send_data();
        this.$root.$refs.Preliminares.send_data();
        this.$root.$refs.Terracerias.send_data();
        this.$root.$refs.Cimentacion.send_data();
        this.$root.$refs.Estructura.send_data();
        this.$root.$refs.Albanileria.send_data();
        this.$root.$refs.Limpieza.send_data();
        this.$root.$refs.Acabados.send_data();
        this.$root.$refs.Hac.send_data();
        this.$root.$refs.Carpinteria.send_data();
        this.$root.$refs.Hidrosanitaria.send_data();
        this.$root.$refs.Electrico.send_data();
        this.$root.$refs.Circuito.send_data();
        this.$root.$refs.Iluminacion.send_data();
        this.$root.$refs.carga_planos.cargar();
        if(this.imagen != null && this.select == true){
          const imagen = Array.prototype.slice.call(this.imagen);
          imagen.forEach((element) => {
          firebase.storage().ref('Proyectos/'+this.proyecto+'/ImagenProyecto/'+element.name).put(element);
          });
        }
        this.$router.replace('/menu');
    },
  }
}
</script>

<style scoped>
.creacion{
  margin: 0%;
  padding: 0%;
}
</style>
