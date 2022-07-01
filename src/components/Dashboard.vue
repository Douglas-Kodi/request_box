<template>
    <div class="plat-table">
        <Message :msg="msg" v-show="msg" />
        <table class="list-table">
            <tr>
                <th>ID</th>
                <th>User name</th>
                <th>Plataforma</th>
                <th>Sugestão</th>
                <th>Tipo</th>
                <th>Valor</th>
                <th>Status</th>
                <th>Remover</th>
            </tr>
            <tr v-for="sugestao in sugestoes" :key="sugestao.id">
                <td>{{ sugestao.id }}</td>
                <td>{{ sugestao.nome }}</td>
                <td>{{ sugestao.tipo }}</td>
                <td>{{ sugestao.sugestao }}</td>
                <td>{{ sugestao.valor }}</td>
                <td>{{ sugestao.preco }}</td>
                <td>
                    <select name="status" id="status" @change="updateSugestao($event, sugestao.id)">
                        <option value="">Selecione</option>
                        <option v-for="s in status" :key="s.id" :value="s.nome" :selected="sugestao.status == s.nome">
                            {{ s.nome }}
                        </option>
                    </select>
                </td>
                <td>
                    <button class="delete-btn" @click="deleteSugestao(sugestao.id)">FF</button>
                </td>
            </tr>
        </table>
    </div>
</template>

<script>
import Message from './Message.vue'

export default {
    name:"Dashboard",
    data(){
        return {
            sugestoes: null,
            sugestao_id: null,
            status: [],
            msg: null
        }
    },
    methods: {
        async getSugestoes() {
            const req = await fetch("http://localhost:3000/sugestoes");

            const data = await req.json();

            this.sugestoes = data;

            this.getStatus();
        },
        async getStatus(){
            const req = await fetch("http://localhost:3000/status");

            const data = await req.json();

            this.status = data;
        },
        async deleteSugestao(id){
            const req = await fetch(`http://localhost:3000/sugestoes/${id}`, {
                method: "DELETE"
            });

            const res = await req.json();
            
            this.msg = `Sugestão removido com sucesso!`;
            
            setTimeout(() => this.msg = "",3000);

            this.getSugestoes();
        },
        async updateSugestao(event, id){
            const option = event.target.value;

            const dataJson = JSON.stringify({status:option});

            const req = await fetch(`http://localhost:3000/sugestoes/${id}`, {
                method: "PATCH",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            });

            const res = await req.json();
            
            this.msg = `Sugestão n° ${res.id} foi atualiuzado para ${res.status}!`;
            
            setTimeout(() => this.msg = "",3000);
        }
    },
    mounted() {
        this.getSugestoes();
    },
    components: {
        Message
    }
}
</script>

<style scoped>
    .plat-table{
    }
    .list-table{
        margin:auto auto;
        border:1px solid #fff;
        width:80%;
        height:80%;
        background-color:rgba(255, 255, 255, 0.8);
    }
    .list-table th{
        color:black;
        font-size: 18px;
        text-align: center;
        vertical-align: middle;
    }
    .list-table td{
        color:black;
        font-size: 16px;
        text-align: center;
        vertical-align: middle;
    }
</style>