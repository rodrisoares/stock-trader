<template>
    <v-toolbar app>
        <v-toolbar-title class="headline text-uppercase mr-4">
            <span>Stock</span>
            <span class="font-weight-light">Trader</span>
        </v-toolbar-title>
        <v-toolbar-items>
            <v-btn flat to="/">Início</v-btn>
            <v-btn flat to="/portfolio">Portfólio</v-btn>
            <v-btn flat to="/stocks">Ações</v-btn>
        </v-toolbar-items>

        <v-spacer></v-spacer>

        <v-toolbar-items>
           
            <v-btn flat @click="endDay">Finalizar Dia </v-btn>
            
            <v-alert color="blue" type="success" :value="finalizarDia">
                Dia finalizado!
            </v-alert>

            <v-alert type="success" :value="salvarDados">
                Dados salvo com sucesso!
            </v-alert>

            <v-alert type="success" :value="carregarDados">
                Dados carregados com sucesso!
            </v-alert>

            <v-menu offset-y>
                <v-btn flat slot="activator">Salvar & Carregar</v-btn>
                <v-list>
                    <v-list-tile @click="saveData">
                        <v-list-tile-title>Salvar Dados</v-list-tile-title>
                    </v-list-tile>
                    <v-list-tile @click="loadDataLocal">
                        <v-list-tile-title>Carregar Dados</v-list-tile-title>
                    </v-list-tile>
                </v-list>
            </v-menu>
            <v-layout align-center>
                <span class="text-uppercase grey--text text--darken-2">
                    Saldo: {{ funds | currency }}
                </span>
            </v-layout>
        </v-toolbar-items>
    </v-toolbar>
</template>

<script>
import { mapActions } from 'vuex'

export default {
    data() {
        return {
            finalizarDia: false,
            salvarDados: false,
            carregarDados:false,
        }
    },
    computed: {
        funds() {
            return this.$store.getters.funds
        }
    },
    methods: {
        ...mapActions(['randomizeStocks', 'loadData']),
        endDay() {
            this.finalizarDia= true;

            setTimeout(()=>{
                this.finalizarDia=false
            },1000)

            this.randomizeStocks()
        },
        saveData() {
            this.salvarDados= true;

            setTimeout(()=>{
                this.salvarDados=false
            },1000)

            const { funds, stockPortfolio, stocks } = this.$store.getters
            this.$http.put('data.json', { funds, stockPortfolio, stocks })

        },
        loadDataLocal() {
            this.carregarDados = true;

            setTimeout(()=>{
                this.carregarDados = false
            },1000)

            this.loadData()
        }
    }
}
</script>

<style>
.v-alert {
    border-color: rgba(0,0,0,0.12) !important;
    position: fixed;
    top: 60px;
    right: 60px;
}
</style>
