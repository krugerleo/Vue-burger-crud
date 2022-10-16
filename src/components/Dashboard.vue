<template>
    <div id="burger-table">
        <Message :msg="msg" v-show="msg" />
        <div>
            <div id="burger-table-heading">
                <div class="order-id">#:</div>
                <div>Clinte:</div>
                <div>Pão</div>
                <div>Carne</div>
                <div>Opcionais</div>
                <div>Ações:</div>
            </div>
        </div>
        <div id="burger-table-rows">
            <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
                <div class="order-number">{{burger.id}}</div>
                <div>{{burger.nome}}</div>
                <div>{{burger.pao}}</div>
                <div>{{burger.pao}}</div>
                <div>
                    <ul>
                        <li v-for="(opcional,index) in burger.opcionais" :key="index">{{opcional}}</li>
                    </ul>
                </div>
                <div>
                    <select name="status" class="status" @change="updateBurger($event, burger.id)">
                        <option value="">Selecione</option>                     
                        <option v-for="s in status" :key="s.id" :value="s.tipo" :selected="burger.status == s.tipo">{{s.tipo}}</option>
                    </select>
                    <button class="delete-btn" @click="deleteBurger(burger.id)">Cancelar</button>
                </div>
                
            </div>
        </div>
    </div>
</template>
<script>
import Message from '@/components/Message.vue'

export default {
    name: 'Dashboard',
    components:{
        Message
    },
    data(){
        return{
            burgers: null,
            status:[],
            msg: null
        }
    },
    mounted(){
        this.getPedidos();
        this.getStatus();
    },
    methods:{
        async getPedidos(){
            const req = await fetch("http://localhost:3000/burgers");
            const response = await req.json();
            this.burgers = response;
        },
        async getStatus(){
            const req = await fetch("http://localhost:3000/status");
            const response = await req.json();
            this.status = response;
        },
        async deleteBurger(id){
            const req = await fetch(`http://localhost:3000/burgers/${id}`,{
                method: "DELETE"
            });
            const response = await req.json();

            // Colocar mensagem de sistema
            this.msg = `Pedido removido com sucesso`;

            setTimeout(()=>this.msg="", 3000);

            this.getPedidos();
        },
        async updateBurger(event, id){
            console.log(event)
            const option = event.target.value;

            const dataJson = JSON.stringify({status: option});

            const req = await fetch(`http://localhost:3000/burgers/${id}`,{
                method: "PATCH",
                headers: {"Content-Type": "application/json"},
                body: dataJson
            });

            const response = await req.json();

            // Colocar mensagem de sistema
            this.msg = `O Pedido nº ${response.id} foi atualizado para ${response.status}`;

            setTimeout(()=>this.msg="", 3000);
        }
    }
    
}
</script>
<style scoped>
  #burger-table {
    max-width: 1200px;
    margin: 0 auto;
  }
  #burger-table-heading,
  #burger-table-rows,
  .burger-table-row {
    display: flex;
    flex-wrap: wrap;
  }
  #burger-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
  }
  .burger-table-row {
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #CCC;
  }
  #burger-table-heading div,
  .burger-table-row div {
    width: 19%;
  }
  #burger-table-heading .order-id,
  .burger-table-row .order-number {
    width: 5%;
  }
  select {
    padding: 12px 6px;
    margin-right: 12px;
  }
  .delete-btn {
    background-color: #222;
    color:#fcba03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
  }
  
  .delete-btn:hover {
    background-color: transparent;
    color: #222;
  }
  
</style>