<template>
    <div id="recovery">
        <br>
        <br>
        <h3>Restablecer la contraseña</h3>
        <form id="formulario" class="form" @submit.prevent="enviar">
            <div>
                <b-form-input id="correo" v-model="correo" placeholder="Correo" :state="null" type="email" />
            </div>
            <br>
            <b-button type="submit" class="btn btn-dark btn-lg btn-block" value="submit">Enviar</b-button>
        </form>
    </div>
</template>
<script>
import firebase from "firebase/app";
import "firebase/app";
import "firebase/auth";
export default{
    data(){
        return{
            correo: null,
        }
    },
    methods:{
        enviar(){//este metodo nos permite enviar un correo al usuario para poder recuperar su contraseña del sistema
            firebase.auth().sendPasswordResetEmail(this.correo)
            .then(() => {
                alert('Se ha enviado un enlace a su correo electronico para restablecer la contraseña');
            }).catch((error) => {
            //var errorCode = error.code;
            var errorMessage = error.message;
                alert(errorMessage);//si el usuario introduce mal el correo
            });
        }
    }
}

</script>
<style scoped>
#recovery{
    background-color: cadetblue;
    background-image: url(../assets/nav.webp);
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
    flex-direction: column;
    justify-content: center;
    width: 30%;
    min-width: 350px;
    max-width: 100%;
    background: rgba(19, 35, 47, 0.8);
    border-radius: 5px;
    padding: 40px;
    box-shadow: 0 6px 10px 4px black;
    margin-top: auto;
}
</style>
