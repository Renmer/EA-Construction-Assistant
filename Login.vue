<!-- Añade todo el codigo html que se visualizara en el componente Login.vue -->
<template>
    <div id="login" fluid alt="Responsive image">
        <br>
        <form id="formulario" class="form" @submit.prevent="iniciar">
            <div class="form-group">
                <b-form-input id="correo" v-model="correo" type="email" placeholder="Correo" :state="null"/>
            </div>

            <div class="form-group">
                <b-form-input id="password" v-model="password" type="password" placeholder="Contraseña" :state="null"/>
            </div>

            <b-button type="submit" class=" btn-dark btn-block" value="submit">Entrar</b-button>
            <p class="forgot-password text-center mt-2 mb-4">
                <b-link to="/recovery">¿Olvidaste la contraseña?</b-link>
            </p>
        </form>
    </div>
</template>
<!-- Etiquet para añadir todo lo referente javascript del componente Login.vue -->
<script>
import firebase from "firebase/app";//definimos las librerias que vamos a utilizara para autenticar al usuario
import "firebase/app";
import "firebase/auth";

export default {
    data(){
        return{
            correo: "",//definimos los campos input vacios para evitar errores
            password: "",
        }
    },
    methods: {
            iniciar: function() {//funcion que valida el correo y contraseña, redirecciona al menu y limpia los campos despues de acceder
                firebase.auth().signInWithEmailAndPassword(this.correo, this.password)
                .then( () => {this.$router.replace('/menu');}).catch(function(error) { alert(error);});
                this.correo = '';
                this.password = '';
            },
    }
}
</script>
<!-- Etiqueta para añadir el estilo al componente Login.vue -->
<style scoped>

h3{
    text-align: center;
    color: black;
}
#login{
    background-image: url(../assets/fondo_login.webp);
    position:absolute; 
    z-index:1; 
    width:100%; 
    height:100%;
    max-width: auto;
    max-height: auto;
    text-align: center;
}
#formulario{
    border: 5px black;
    margin: 3rem auto;
    display: flex;
    /*align-items: center;*/
    flex-direction: column;
    justify-content: center;
    width: 30%;
    min-width: 350px;
    max-width: 100%;
    background: rgba(19, 35, 47, 0.8);
    border-radius: 5px;
    padding: 50px;
    box-shadow: 0 7px 10px 4px rgba(0, 0, 0, 0.3);
    margin-top: auto;
}

</style>
