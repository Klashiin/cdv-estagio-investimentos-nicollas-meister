<template>
    <main>
        <div id="app">
            <div>
                <h1>Lista de Clientes</h1>
                <ul>
                    <li v-for="cliente of clientes" :key="cliente.id">
                        {{ cliente.nome }}
                        <button @click="() => toggle('buttonTrigger')">
                            EDITAR
                        </button>
                    </li>
                </ul>
            </div>
            <div>
                <h2>Adicionar Cliente</h2>
                <button @click="() => toggle('buttonTrigger2')">
                    ADICIONAR CLIENTE
                </button>
            </div>
            <popup v-if="trigger.buttonTrigger" :toggle="() => toggle('buttonTrigger')">
                <h2>Editar Cliente</h2>
            </popup>
            <popup style="text-align:left" v-if="trigger.buttonTrigger2" :toggle="() => toggle('buttonTrigger2')">
                <h2>Adicionar Cliente</h2>
                <form>
                    <label for="nome">Nome:
                        <input id="nome" type="text" placeholder="Nome do cliente"> 
                    </label>
                    <br>
                    <label for="cpf">CPF:
                        <input id="cpf" type="text" placeholder="CPF do cliente">
                    </label>
                    <br>
                    <label for="etapa1">Etapas: <br>
                        <input id="etapa1" type="radio" name="etapas">Aguardando assinatura de documentos
                    </label>
                    <br>
                    <label for="etapa2">
                        <input id="etapa2" type="radio" name="etapas">Aguardando transferência de recursos
                    </label>
                    <br>
                    <label for="etapa3">
                        <input id="etapa3" type="radio" name="etapas">Gestão de patrimônio ativa
                    </label>
                    
                </form>
            </popup>
        </div>
    </main>
</template>

<script>
import Popup from "./components/Popup";
import axios from "axios";
import { ref } from "vue";
export default {
    components: { Popup },
    name: "App",
    setup() {
        const trigger = ref({ buttonTrigger: false, buttonTrigger2:false });
        const toggle = (t) => {
            trigger.value[t] = !trigger.value[t];
        };
        return {
            Popup,
            trigger,
            toggle,
        };
    },
    data() {
        return {
            clientes: [],
        };
    },
    async created() {
        try {
            const res = await axios.get(`http://localhost:3000/clientes`);

            this.clientes = res.data;
        } catch (e) {
            console.error(e);
        }
    },
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
ul {
    display: table;
    margin-right: auto;
    margin-left: auto;
}
</style>
