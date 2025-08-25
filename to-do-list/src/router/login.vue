
    <template>

        <div class="container">

        <h1 id="title">WELCOME <span style="color: black; font-size: 62px ;">BACK</span></h1>

        <h2 id="subtitle">Sing in</h2>

  <div class="form">

    <div class="input-wrapper">
      <input type="email" v-model="email" placeholder="email" class="inputs">
      <img :src="email_logo" alt="" id="email-logo">
    </div>

    <div class="input-wrapper">
      <input ref="senhaInput" type="password" v-model="senha" placeholder="senha" class="inputs">
      <showpassword class="show" :input="$refs.senhaInput"></showpassword>
    </div>

    <div class="to-register-div">
     <p id="to-register">novo aqui?,</p>
     <p> <router-link id="router-link" to="register">Cadastrar-se</router-link></p>
    </div>

    <button @click="fetchSend" class="bt">Sign in</button>
    <div id="estado-container">
    <p v-if="estado === true" id="estado">{{ estado_dado }}</p>
    </div>
  </div>

</div>
</template>




<script>

import showpassword from '@/components/showpassword.vue'
import email_logo from '@/assets/icones/envelope (2).png'



export default{
    data(){
    return{
        email: '',
        senha: '',
        estado_dado: undefined,
        estado : false,
        email_logo

    }
  },
    methods: {
        fetchSend(){

            const token = localStorage.getItem('token')

            if(token){
                        this.$router.push("/");
                    }

            const dados = {
                email: this.email,
                senha: this.senha
            }

            if(!dados.email ||!dados.senha){
                this.estado_dado = 'por favor prencha todos os campos!'
                this.estado = true;
               return console.log(this.estado_dado);
            }

            this.estado = false;

            (async () => {
                    try {
                        const response = await fetch(`${import.meta.env.VITE_API_URL}/login`, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(dados)
                        });

                        const resultado = await response.json();
                        console.log(resultado);

                        if (!response.ok) {
                                        this.estado_dado = resultado.message || 'Usuário inválido, email ou senha incorretos!';
                                        this.estado = true;
                                        return;
                            }


                        this.$router.push("/");

                        localStorage.setItem('token', resultado.token)

                    
                        
                    } catch (erro) {
                        console.error('Erro na requisição:', erro);
                    }

              
                    })();


        }
    
    },

    components: {
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
    display: flexbox;
    justify-content: center;
    flex-direction: row;
    position: relative;
    left: 40px;
    width: 390px;
}

.container{
    display: flex;
    align-items: center;
    justify-content: start;
    flex-direction: column;
    width: 100%;
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
    position: relative;
    top: 140px;
}

.input-wrapper {
  position: relative;
  width: fit-content;
  width: 335px; /* igual ao input *//* espaço para o ícone de mostrar senha */
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
  right: -7px;
  transform: translateY(-50%);
  width: 20px;
  height: 20px;
  pointer-events: none; /* não atrapalha clique */
}

.show{
  position: absolute;
  top: 50%;
  right: -7px; /* distância da borda direita */
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
}

 
</style>
