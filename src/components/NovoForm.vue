<template>
    <div>
        <form action="" @submit="enviarFormulario($event)">
            <div>
                <input type="text" v-model="nome">
            </div>
            <div>
                <input type="text" v-model="sobrenome">
            </div>
            <div>
                <button type="submit">Enviar</button>
            </div>
        </form>

        <p>O nome da empresa é {{empresa }}</p>

    </div>

</template>

<script>
import axios from 'axios';

    export default{
        name: 'NovoForm',
        data(){
            return{
                nome: '',
                sobrenome: '',
                empresa: 'Ok'

            }
           
        },
            methods:{
                enviarFormulario(e) {
                    e.preventDefault();
                    
                    // Envia a solicitação POST para enviar os dados para o back-end
                    const formData = {
                        nome: this.nome,
                        sobrenome: this.sobrenome
                    };

                    axios.post('http://localhost:3000/api', formData)
                        .then(response => {
                        console.log(response.data);
                        
                        // Após a resposta bem-sucedida do back-end, faça uma solicitação GET para obter os dados do back-end
                        axios.get('http://localhost:3000/api')
                            .then(response => {
                            const empresa = response.data.empresa;
                            console.log(empresa);
                            
                            // Atualize a variável "empresa" no front-end com os dados recebidos do back-end
                            this.empresa = empresa;
                            })
                            .catch(error => {
                            console.error(error);
                            });
                        })
                        .catch(error => {
                        console.error(error);
                        });
                    }
        },
        mounted() {
    // Faça uma solicitação GET para obter os dados iniciais do back-end ao montar o componente
    axios.get('http://localhost:3000/api')
      .then(response => {
        const empresa = response.data.empresa;
        console.log(empresa);
        
        // Atualize a variável "empresa" no front-end com os dados recebidos do back-end
        this.empresa = empresa;
      })
      .catch(error => {
        console.error(error);
      });
  }
    }
  
</script>
