<template>

    


    <div class="container">

    <h1 id="title">WELCOME</h1>
    <h2 id="subtitle">Sing Up</h2>
    <div class="form">
    <div class="input-wrapper">
    <input type="text" v-model="name" placeholder="name" class="inputs">
    <img :src="user_logo" alt="" class="show">
    </div>
    <div class="input-wrapper">
    <input type="email" v-model="email" placeholder="email" class="inputs">
    <img :src="email_logo" alt="" class="show">
    </div>
    <div class="input-wrapper">
    <input type="password" v-model="senha" placeholder="senha" class="inputs" ref="password">
    <showpassword :input="$refs.password" class="show"/>
    </div>
    <div class="to-register-div">
     <p id="to-register">Ja tem uma Conta?,</p>
     <p> <router-link id="router-link" to="login">Login</router-link></p>
    </div>
    <button @click="fetchSend" class="bt">sing in</button>
    <p v-if="estado === true" id="estado">{{ estado_dado }}</p>
    </div>
    </div>
</template>

<script>

import showpassword from '@/components/showpassword.vue'
import email_logo from '@/assets/icones/envelope (2).png'
import user_logo from '@/assets/icones/user.png'


export default{

  
    

    data(){
    return{
        password: '',
        name: '',
        email: '',
        senha: '',
        estado_dado: undefined,
        estado : false,
        email_logo,
        user_logo

    }
  },
    methods: {
        fetchSend(){

            const token = localStorage.getItem('token')

            if(token){
                this.$router.push("/");
            }

            const dados = {
                name: this.name,
                email: this.email,
                senha: this.senha
            }

            if(!dados.name || !dados.email ||!dados.senha){
                this.estado_dado = 'por favor prencha todos os campos!'
                this.estado = true;
               return console.log(this.estado_dado);
            }

            this.estado = false;



            (async () => {
                    try {
                        const response = await fetch(`${import.meta.env.VITE_API_URL}/register`, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(dados)
                        });

                        const resultado = await response.json();

                        localStorage.setItem('token', resultado.token)
                        console.log(resultado);
                        
                        if (!response.ok) {
                                        this.estado_dado = resultado.message || 'Usuário inválido, email ou senha incorretos!';
                                        this.estado = true;
                                        return;
                            }

                        if(resultado.ok){
                        this.$router.push("/");
                        }
                    } catch (erro) {
                        console.error('Erro na requisição:', erro);
                    }
                    })();


        }
    },

    components:{
        showpassword
    }

}
</script>


<style>

@import url('https://fonts.googleapis.com/css2?family=Audiowide&family=Dangrek&family=Karla:ital,wght@0,200..800;1,200..800&family=Open+Sans:ital,wght@0,300..800;1,300..800&family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Roboto+Condensed:ital,wght@0,100..900;1,100..900&display=swap');

body{
    background-color: rgb(21, 21, 21);
}

#estado-container{
    display: flex;
    justify-content: center;
    flex-direction: row;
    min-width: 400px;
    max-height: 40px;
    
 }
 
.container{
    display: flex;
    align-items: center;
    justify-content: start;
    flex-direction: column;
    min-width: 100%;
    height: 98vh;
}



.inputs::placeholder{
    color: black;
    font-size: 16px;
    font-family: "Poppins", sans-serif;
    font-weight: 600;
}

.bt{
    width: 354px;
    height: 48px;
    border: none;
    background-color: rgb(15, 15, 15);
    box-shadow: rgba(110, 110, 110, 0.05) 0px 6px 24px 0px, rgba(138, 138, 138, 0.08) 0px 0px 0px 1px;
    font-family: "Poppins", sans-serif;
    font-weight: 600;
    color: white;
    border-radius: 3px;
}

.form{
    display: flex;
    gap: 3px;
    flex-direction: column;
    align-items: center;
    width: auto;
}

.input-wrapper {
  position: relative;
  width: fit-content; /* igual ao input *//* espaço para o ícone de mostrar senha */
}

#estado{
    color: red;
    font-family: "Poppins", sans-serif;
    font-size: 15px;
    position: relative;
    top: -10px;
}

.inputs {
  margin-top: 5px;
  margin-bottom: 5px;
  width: 335px;
  height: 48px;
  border-radius: 2px;
  border: none;
  outline: none;
  padding-left: 16px;
  font-weight: 600;
  font-size: 14px;
  font-family: "Poppins", sans-serif; /* espaço pro ícone da esquerda */
}

#email-logo {
  position: absolute;
  top: 50%;
  right: -2px;
  transform: translateY(-50%);
  width: 20px;
  height: 20px;
  pointer-events: none; /* não atrapalha clique */
}

.show{
  position: absolute;
  top: 50%;
  right: 7px; /* distância da borda direita */
  transform: translateY(-50%);
  width: 21px;
  height: 21px;
  cursor: pointer;
}

#router-link{
    color: white;
    font-family: "Poppins", sans-serif;
}

#to-register{
    color: white;
    font-family: "Poppins", sans-serif;
    margin-right: 16px;
    position: relative; left: 10px;
}

.to-register-div{
    position: relative;bottom: 23px;
    display: inline-flex;
    max-height: 20px;
    margin-top: -1px;
    margin-bottom: -3px;
    
}


#title{
    color: white;
    font-family: "Poppins", sans-serif;
    font-size: 59px;
    position: relative;
    top: 130px;

}

#subtitle{
    color: white;
    font-family: "Poppins", sans-serif;
    font-size: 40px;
    position: relative;
    top: 120px;
    margin: 0;
    margin-bottom: 140px;
}

 
</style>

