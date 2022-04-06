<template>
    <div id="registro">
        <br>
        <br>
        <form id="formulario" class="form" @submit.prevent="registro">
        <div class="form-group">
            <b-form-input id="correo" v-model="correo" type="email" placeholder="Correo" :state="null" required/>
            <br>
            <b-form-input id="password" v-model="password" type="password" placeholder="Contraseña" :state="null" required/>
            <br>
            <b-form-input id="password_confirmacion" v-model="password_confirmacion" type="password" placeholder="Confirmacion contraseña" :state="null" required/>
        </div>
        <br>
        <b-button type="submit" class="btn btn-dark btn-lg btn-block" value="submit">Entrar</b-button>
        </form>
    </div>
</template>
<script>
import firebase from "firebase/app";
import "firebase/app";
import "firebase/auth";

export default {
    data(){
        return{
                correo: null,
                password: null,
                password_confirmacion: null,
        }
    },
    methods: {
            registro: function() {//valida que todos los datos que ingrese el usuario sean validos
                    if(this.validaCorreo(this.correo) == true){
                        if(this.password == this.password_confirmacion && this.password != null && this.password.length >=8){
                            firebase.auth().createUserWithEmailAndPassword(this.correo,this.password);
                            alert('Usuario Creado Correctamente');
                            this.$router.replace('/menu');
                            this.limpiaCampos();
                        }
                        else{
                            alert('La contraseña no es igual o es menor a 8 caracteres');
                        }
                    }
                    else{
                        alert('El correo electronico no es valido');
                    }

            },
            validaCorreo: function (email) {
            var re = /\w+@\w+\.+[a-z]/; //expresion regular para validar que el correo electronico cuente con la estructura adecuada
            return re.test(email);
            },
            limpiaCampos: function(){
                this.nombre = null,
                this.correo= null,
                this.password = null,
                this.password_confirmacion = null
            }
    },
}
</script>
<style scoped>
h3{
    text-align: center;
}
#registro{
    background-image: url(../assets/registro.jpg);
    background-size: 100%;
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
