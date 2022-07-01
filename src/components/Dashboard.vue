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
                    <button class="delete-btn" @click="deleteSugestao(sugestao.id)">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24"><path fill-rule="evenodd" d="M5.72 5.72a.75.75 0 011.06 0L12 10.94l5.22-5.22a.75.75 0 111.06 1.06L13.06 12l5.22 5.22a.75.75 0 11-1.06 1.06L12 13.06l-5.22 5.22a.75.75 0 01-1.06-1.06L10.94 12 5.72 6.78a.75.75 0 010-1.06z"></path></svg>
                    </button>
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
            var id_alert = id;
            const req = await fetch(`http://localhost:3000/sugestoes/${id}`, {
                method: "DELETE"
            });

            const res = await req.json();
            
            this.msg = `Sugestão n° ${id_alert} removido com sucesso!`;
            
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
        overflow: auto;
    }
    .list-table{
        margin:auto auto;
        width:80%;
        height:712px;
        background-color:rgba(255, 255, 255, 1);
        border-radius: 5px;
        border-collapse: collapse;
        border-spacing: 0;
    }
    .list-table th, td{
        padding: 10px;
    }
    .list-table th{
        padding-top: 12px;
        padding-bottom: 12px;
        color: white;
        font-size: 18px;
        text-align: center;
        vertical-align: middle;
        background-color: rgb(10, 145, 133);
    }
    .list-table td{
        color:black;
        font-size: 16px;
        text-align: center;
        vertical-align: middle;
    }
    .list-table select{
        color:black;
        font-size: 16px;
        text-align: center;
        vertical-align: middle;
    }
    .list-table tr:nth-child(even){
        background-color: #f2f2f2;
    }
    .list-table tr:hover {
        background-color: #ddd;
    }
</style>