<template>
    <div class="form">
        <h2>Caixa de Sugestão para Jogos</h2>
        <Message :msg="msg" v-show="msg" />
        <form @submit="createRequest">
            <div>
                <label for="nome">User Name (Nick Name):</label>
            </div>
            <div>
                <input type="text" name="nome" v-model="nome" placeholder="Digite seu username..."/>    
            </div>
            <div>
                <label for="tipo">Plataforma:</label>
            </div>
            <div>
                <select name="tipo" id="tipo" v-model="tipo" required>
                    <option value="">Selecione a plataforma do jogo!</option>
                    <option v-for="tipo in tipos" :key="tipo.id" :value="tipo.nome">
                        {{ tipo.nome }}
                    </option>
                </select>
            </div>
            <div>
                <label for="sugestao">Sugestão:</label>
            </div>
            <div>
                <input type="text" name="sugestao" v-model="sugestao" placeholder="Digite sua sugestão de jogo..." required/>
            </div>
            <div>
                <label for="valor">Valor:</label>
                <section v-for="variavel in variaves" :key="variavel.id">
                    <input type="radio" name="variavel" :value="variavel.nome" v-model="valor" required>
                    <span>{{ variavel.nome }}</span>
                </section>
            </div>
            <div>
                <input type="text" v-if="valor=='Pago'" name="preco" v-model="preco" placeholder="Digite o valor do jogo..." required />
                <input type="text" v-else name="preco" v-model="preco" placeholder="Free" disabled />
            </div>    
            <div>
                <input type="submit" class="submit-btn" value="Adicionar" />
            </div>    
        </form>    
    </div>    
</template>

<script>
import Message from './Message.vue'

export default {
    name: "Form",
    data() {
        return {
            tipos: null,
            variaves: null,
            nome: null,
            tipo: null,
            sugestao: null,
            valor: null,
            preco: null,
            msg: null
        }
    },
    methods: {
        async getJogos(){
            const req = await fetch("http://localhost:3000/jogos");
            const data = await req.json();

            this.tipos = data.tipos;
            this.variaves = data.variaves;
        },
        async createRequest(e){

            e.preventDefault();

            const data = {
                nome: this.nome,
                tipo: this.tipo,
                sugestao: this.sugestao,
                valor: this.valor,
                preco: this.preco,
                status: "Pendente"
            }

            const dataJson = JSON.stringify(data);

            const req = await fetch("http://localhost:3000/sugestoes",{
                method: "POST",
                headers: {"Content-Type": "application/json"},
                body: dataJson
            });

            const res = await req.json();

            this.msg = `Jogo n° ${res.id} adicionado com sucesso!`;
            
            setTimeout(() => this.msg = "",3000);

            this.nome = "";
            this.tipo = "";
            this.sugestao = "";
            this.valor = "";
            this.preco = "";

        }
    },
    mounted() {
        this.getJogos()
    },
    components: {
        Message
    }
}
</script>

<style scoped>
    h2{
        color:white;
        text-align:center;
        padding:20px;
    }
    .form{
        margin: auto auto;
        padding:20px 20px;
        width:80%;
        border:0px solid black;
    }
    label, span{
        color:white;
        padding:15px 15px;
        font-size: 18px;
        display: inline-block;
    }
    input[type=text], select{
        width:100%;
        padding: 10px 10px;
        border-radius:5px;
    }
    input:disabled{
        background-color:gray;
    }
    input:disabled::placeholder{
        color:white;
    }
    .submit-btn{
        width:100px;
        margin:60px;
        padding:10px 10px;
        border-radius:5px;
        float:right;
    }
    .submit-btn:hover{
        transition:0.5s;
        color:green;
    }
</style>