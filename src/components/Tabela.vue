<template>
    <div class="root">
        <div class="tabela-jogos">
            <table>
                <thead>
                <th>Tabela jogos</th>
                </thead>
                <tbody>
                    <Rodada ref="rodada1"  :rodada="1"  v-bind:participantes="participantes" v-bind:ordem="[0, 1, 2, 3, 4, 5, 6, 7]"></Rodada>
                    <Rodada ref="rodada2"  :rodada="2"  v-bind:participantes="participantes" v-bind:ordem="[0, 2, 1, 3, 4, 6, 5, 7]"></Rodada>
                    <Rodada ref="rodada3"  :rodada="3"  v-bind:participantes="participantes" v-bind:ordem="[0, 3, 1, 2, 4, 7, 5, 6]"></Rodada>
                    <Rodada ref="rodada4"  :rodada="4"  v-bind:participantes="participantes" v-bind:ordem="[0, 4, 1, 5, 2, 6, 3, 7]"></Rodada>
                    <Rodada ref="rodada5"  :rodada="5"  v-bind:participantes="participantes" v-bind:ordem="[0, 5, 1, 4, 2, 7, 3, 6]"></Rodada>
                    <Rodada ref="rodada6"  :rodada="6"  v-bind:participantes="participantes" v-bind:ordem="[0, 6, 1, 7, 2, 4, 3, 5]"></Rodada>
                    <Rodada ref="rodada7"  :rodada="7"  v-bind:participantes="participantes" v-bind:ordem="[0, 7, 1, 6, 2, 5, 3, 4]"></Rodada>
                    <Rodada ref="rodada8"  :rodada="8"  v-bind:participantes="participantes" v-bind:ordem="[1, 0, 3, 2, 5, 4, 7, 6]"></Rodada>
                    <Rodada ref="rodada9"  :rodada="9"  v-bind:participantes="participantes" v-bind:ordem="[2, 0, 3, 1, 6, 4, 7, 5]"></Rodada>
                    <Rodada ref="rodada10" :rodada="10" v-bind:participantes="participantes" v-bind:ordem="[3, 0, 2, 1, 7, 4, 6, 5]"></Rodada>
                    <Rodada ref="rodada11" :rodada="11" v-bind:participantes="participantes" v-bind:ordem="[4, 0, 5, 1, 6, 2, 7, 3]"></Rodada>
                    <Rodada ref="rodada12" :rodada="12" v-bind:participantes="participantes" v-bind:ordem="[5, 0, 4, 1, 7, 2, 6, 3]"></Rodada>
                    <Rodada ref="rodada13" :rodada="13" v-bind:participantes="participantes" v-bind:ordem="[6, 0, 7, 1, 4, 2, 5, 3]"></Rodada>
                    <Rodada ref="rodada14" :rodada="14" v-bind:participantes="participantes" v-bind:ordem="[7, 0, 6, 1, 5, 2, 4, 3]"></Rodada>
                </tbody>
            </table>
        </div>
        <div class="main">
            <div class="divisao-atual">
                <h1>{{divisaoAtual}}º Divisão</h1>    
                <table style="display: inline">
                    <thead>
                    <th class="first-column">Rodadas</th>
                    <th v-for="n in [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14]" v-bind:key = n  v-bind:class="{'border-right': n==7}"> {{n}}º</th>
                    </thead>
                    <tbody>
                        <tr v-for="(participante, index) in participantes" v-bind:key="index" >
                            <td class="first-column">
                                {{participante.nome}}
                            </td>
                            <td v-for="(pontuacao, index) in participante.acumulado" v-bind:class="{'border-right': index==6}" v-bind:key="index" > {{pontuacao}} </td>

                        </tr>
                    </tbody>
                </table>
            </div>

            <div class="tabela-divisoes">
                <table v-for="d in [1, 2, 3, 4]" v-bind:key = d>
                    <thead>
                    <th colspan=3>{{d}}º Divisão</th>

                    </thead>
                    <tbody>
                        <tr v-for="(p, index) in divisaoOrdenada(d)" v-bind:key = index>
                            <td v-if="!editarParticipantes" v-bind:class="p.rotulo">
                                {{p.nome}}
                            </td>
                            <td v-if="editarParticipantes" v-bind:class="p.rotulo">
                                <input class="input-participante" type="text" v-model="p.nome">
                            </td>
                            <td> 
                                <i v-if="p.rotulo == 'subiu'" class="fa fa-angle-up"></i>
                                <i v-if="p.rotulo == 'caiu'" class="fa fa-angle-down"></i>
                                
                            </td>
                            <td>
                                <i v-if="p.posicao == 1" class="fa fa-trophy" style="color: gold;"></i>
                                <i v-if="p.posicao == 2" class="fa fa-trophy" style="color: silver;"></i>
                                <i v-if="p.posicao == 3" class="fa fa-trophy" style="color: #cd7f32;"></i>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>

                <div class="classificacao">
                    <table>
                        <thead>
                        <th width = 160px>Classificação</th>
                        <th width = 70px>Pontos</th>
                        <th width = 120px>Pontos fora</th>
                        </thead>
                        <tbody>
                            <tr v-for="(participante, index) in classificacaoOrdenada()" v-bind:key="index">
                                <td>
                                    {{participante.nome}}
                                </td>
                                <td>
                                    {{participante.total}}
                                </td>
                                <td>
                                    {{participante.pontuacaoFora}}
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
                <div class="dado-area">
                    <div class="dado-container">
                        <button v-if="!mostrarBotaoProximaDivisao" v-on:click="rolarJogada">Rolar dado</button>
                        <button v-if="mostrarBotaoProximaDivisao" v-on:click="processarNovaDivisao">Continuar</button>
                        <br/>
                        <div id="dado" class="dado"></div>
                    </div>
                </div>
                <div class="editar-container" > 
                    <button style="width: 280px" v-on:click="toggleEditarParticipantes">
                        {{editarParticipantes? 'OK' : 'Editar participantes'}} 
                    </button><br/>
                    <div class="reservas-form" v-if="editarParticipantes">
                        <span>
                            <h1 style="margin-top: 5px; margin-bottom: 5px;">Reservas 
                            <a style="margin: 0px 5px;" v-on:click="addReserva"> <i class="fa fa-plus-circle"></i> </a> 
                            <a v-on:click="removeReserva"> <i class="fa fa-minus-circle"></i> </a></h1>
                        </span>
                        <input v-for="(p, index) in reservas" v-bind:key = "index" class="input-participante" type="text" v-model="p.nome">
                    </div>
                </div>
            <br/>
        </div>
    </div>
</template>
<script>
    import Rodada from './Rodada'
    import Vue from 'vue'
    export default {
        name: "Tabela",
        props: [ ],
        components: {
            Rodada
        },
        data() {
            return{
                rodadaAtual: 1,
                partidaAtual: 1,
                jogadorAtual: 1,
                divisaoAtual: 1,
                mostrarBotaoProximaDivisao: false,
                dado: 0,
                dadoGirando: false,
                stop: false,
                iniciado: false,
                editarParticipantes: false,
                participantes: [
                    {"id": 1, "nome": "D1_Jogador1", "pontuacao": [], "total": 0, "pontuacaoFora": 0, "acumulado": ["", "", "", "", "", "", "", "", "", "", "", "", "", ""]},
                    {"id": 2, "nome": "D1_Jogador2", "pontuacao": [],"total": 0, "pontuacaoFora": 0, "acumulado": ["", "", "", "", "", "", "", "", "", "", "", "", "", ""]},
                    {"id": 3, "nome": "D1_Jogador3", "pontuacao": [], "total": 0, "pontuacaoFora": 0, "acumulado": ["", "", "", "", "", "", "", "", "", "", "", "", "", ""]},
                    {"id": 4, "nome": "D1_Jogador4", "pontuacao": [],"total": 0, "pontuacaoFora": 0, "acumulado": ["", "", "", "", "", "", "", "", "", "", "", "", "", ""]},
                    {"id": 5, "nome": "D1_Jogador5", "pontuacao": [], "total": 0, "pontuacaoFora": 0, "acumulado": ["", "", "", "", "", "", "", "", "", "", "", "", "", ""]},
                    {"id": 6, "nome": "D1_Jogador6", "pontuacao": [], "total": 0, "pontuacaoFora": 0, "acumulado": ["", "", "", "", "", "", "", "", "", "", "", "", "", ""]},
                    {"id": 7, "nome": "D1_Jogador7", "pontuacao": [], "total": 0, "pontuacaoFora": 0, "acumulado": ["", "", "", "", "", "", "", "", "", "", "", "", "", ""]},
                    {"id": 8, "nome": "D1_Jogador8", "pontuacao": [], "total": 0, "pontuacaoFora": 0, "acumulado": ["", "", "", "", "", "", "", "", "", "", "", "", "", ""]},
                ],
                reservas: [
                    { nome: "Reserva1" },
                    { nome: "Reserva2" },
                    { nome: "Reserva3" },
                    { nome: "Reserva4" },
                ],
                divisoes: [
                    {participantes: [
                        {"nome": "D1_Jogador1", "posicao": 0},
                        {"nome": "D1_Jogador2", "posicao": 0},
                        {"nome": "D1_Jogador3", "posicao": 0},
                        {"nome": "D1_Jogador4", "posicao": 0},
                        {"nome": "D1_Jogador5", "posicao": 0},
                        {"nome": "D1_Jogador6", "posicao": 0},
                        {"nome": "D1_Jogador7", "posicao": 0},
                        {"nome": "D1_Jogador8", "posicao": 0},
                    ]},
                    {participantes: [
                        {"nome": "D2_Jogador1", "posicao": 0},
                        {"nome": "D2_Jogador2", "posicao": 0},
                        {"nome": "D2_Jogador3", "posicao": 0},
                        {"nome": "D2_Jogador4", "posicao": 0},
                        {"nome": "D2_Jogador5", "posicao": 0},
                        {"nome": "D2_Jogador6", "posicao": 0},
                        {"nome": "D2_Jogador7", "posicao": 0},
                        {"nome": "D2_Jogador8", "posicao": 0},
                    ]},
                    {participantes: [
                        {"nome": "D3_Jogador1", "posicao": 0},
                        {"nome": "D3_Jogador2", "posicao": 0},
                        {"nome": "D3_Jogador3", "posicao": 0},
                        {"nome": "D3_Jogador4", "posicao": 0},
                        {"nome": "D3_Jogador5", "posicao": 0},
                        {"nome": "D3_Jogador6", "posicao": 0},
                        {"nome": "D3_Jogador7", "posicao": 0},
                        {"nome": "D3_Jogador8", "posicao": 0},
                    ]},

                    { participantes: [
                        {"nome": "D4_Jogador1", "posicao": 0},
                        {"nome": "D4_Jogador2", "posicao": 0},
                        {"nome": "D4_Jogador3", "posicao": 0},
                        {"nome": "D4_Jogador4", "posicao": 0},
                        {"nome": "D4_Jogador5", "posicao": 0},
                        {"nome": "D4_Jogador6", "posicao": 0},
                        {"nome": "D4_Jogador7", "posicao": 0},
                        {"nome": "D4_Jogador8", "posicao": 0},
                    ]}
                ]
            }
        },
        methods: {
            sleep(milliseconds) {
            const date = Date.now();
            let currentDate = null;
            do {
                currentDate = Date.now();
            } while (currentDate - date < milliseconds);
            },

            async criarSequenciaAnimacao(){
                const dado = document.getElementById("dado")
                const criarAnimacaoPasso = function(){
                    dado.className = 'dado'
                    let result = Math.floor(Math.random() * 6) + 1
                    dado.classList.add('d'+result)
                    return result
                }
                const delay = async function(ms) {
                    return await new Promise(resolve => setTimeout(resolve, ms));
                }
                for(let i = 0; i < 6; i++){
                    criarAnimacaoPasso()
                    await delay(70);
                }
                return criarAnimacaoPasso()
            },

            async rolarDado(){
                let valor = await this.criarSequenciaAnimacao();
                return valor;
            },

           async rolarJogada(){
               //while(this.stop === false){
                    if(this.rodadaAtual < 15){
                        this.dado = await this.rolarDado();
                        this.$refs["rodada" + this.rodadaAtual].enviarDado(this.dado, this.partidaAtual, this.jogadorAtual);
                        this.jogadorAtual = 3 - this.jogadorAtual;
                        if(this.jogadorAtual == 1){
                            this.partidaAtual++;
                            if(this.partidaAtual == 5){
                                this.partidaAtual = 1;
                                this.rodadaAtual++;
                            }
                        }
                    }
                    if(this.rodadaAtual == 15){
                        let participantesOrdenados = this.classificacaoOrdenada();
                        for(let i = 0; i < 8; i++){
                            this.divisoes[(this.divisaoAtual + 3) % 4].participantes.find(x => x.nome == participantesOrdenados[i].nome).posicao = i + 1;
                        }
                        this.mostrarBotaoProximaDivisao = true;
                        this.divisaoAtual++;
                    }
               //}
           },

           classificacaoOrdenada(){
               return this.participantes.concat().sort(function(a, b){
                    if(a.total == b.total)
                        return b.pontuacaoFora - a.pontuacaoFora;
                    return b.total - a.total
               });
           },

           divisaoOrdenada(divisao){
               return  this.divisoes[divisao - 1].participantes.concat().sort(function(a, b){
                    if(a.posicao == b.posicao)
                        return 0;
                    return a.posicao - b.posicao
               });
           },

           processarDivisoesProximaTemporada(){
                for(let d of this.divisoes){
                   for(let p of d.participantes){
                       p.rotulo = "";
                   }
               }

               var swap =  this.divisoes[0].participantes.find(x => x.posicao == 7).nome;
               this.divisoes[0].participantes.find(x => x.posicao == 7).nome = this.divisoes[1].participantes.find(x => x.posicao == 1).nome;
               this.divisoes[1].participantes.find(x => x.posicao == 1).nome = swap;

               swap =  this.divisoes[0].participantes.find(x => x.posicao == 8).nome;
               this.divisoes[0].participantes.find(x => x.posicao == 8).nome = this.divisoes[1].participantes.find(x => x.posicao == 2).nome;
               this.divisoes[1].participantes.find(x => x.posicao == 2).nome = swap;

               this.divisoes[0].participantes.find(x => x.posicao == 7).rotulo = "subiu"
               this.divisoes[0].participantes.find(x => x.posicao == 8).rotulo = "subiu"
               this.divisoes[1].participantes.find(x => x.posicao == 1).rotulo = "caiu";
               this.divisoes[1].participantes.find(x => x.posicao == 2).rotulo = "caiu";

               swap =  this.divisoes[1].participantes.find(x => x.posicao == 7).nome;
               this.divisoes[1].participantes.find(x => x.posicao == 7).nome = this.divisoes[2].participantes.find(x => x.posicao == 1).nome;
               this.divisoes[2].participantes.find(x => x.posicao == 1).nome = swap;

               swap =  this.divisoes[1].participantes.find(x => x.posicao == 8).nome;
               this.divisoes[1].participantes.find(x => x.posicao == 8).nome = this.divisoes[2].participantes.find(x => x.posicao == 2).nome;
               this.divisoes[2].participantes.find(x => x.posicao == 2).nome = swap;

               this.divisoes[1].participantes.find(x => x.posicao == 7).rotulo = "subiu"
               this.divisoes[1].participantes.find(x => x.posicao == 8).rotulo = "subiu"
               this.divisoes[2].participantes.find(x => x.posicao == 1).rotulo = "caiu";
               this.divisoes[2].participantes.find(x => x.posicao == 2).rotulo = "caiu";

               swap =  this.divisoes[2].participantes.find(x => x.posicao == 7).nome;
               this.divisoes[2].participantes.find(x => x.posicao == 7).nome = this.divisoes[3].participantes.find(x => x.posicao == 1).nome;
               this.divisoes[3].participantes.find(x => x.posicao == 1).nome = swap;

               swap =  this.divisoes[2].participantes.find(x => x.posicao == 8).nome;
               this.divisoes[2].participantes.find(x => x.posicao == 8).nome = this.divisoes[3].participantes.find(x => x.posicao == 2).nome;
               this.divisoes[3].participantes.find(x => x.posicao == 2).nome = swap; 

               this.divisoes[2].participantes.find(x => x.posicao == 7).rotulo = "subiu"
               this.divisoes[2].participantes.find(x => x.posicao == 8).rotulo = "subiu"
               this.divisoes[3].participantes.find(x => x.posicao == 1).rotulo = "caiu";
               this.divisoes[3].participantes.find(x => x.posicao == 2).rotulo = "caiu";
               
               if(this.reservas.length > 0){
                var random1 = Math.floor(Math.random() * this.reservas.length); 
                
                swap =  this.divisoes[3].participantes.find(x => x.posicao == 8).nome;
                this.divisoes[3].participantes.find(x => x.posicao == 8).nome = this.reservas[random1].nome;
                this.reservas[random1].nome = swap;
                this.divisoes[3].participantes.find(x => x.posicao == 8).rotulo = "subiu"
                if(this.reservas.length >= 2){
                    var random2 = 0;
                    do{
                        random2 = Math.floor(Math.random() * this.reservas.length); 
                    }while(random1 == random2);
                    swap =  this.divisoes[3].participantes.find(x => x.posicao == 7).nome;
                    this.divisoes[3].participantes.find(x => x.posicao == 7).nome = this.reservas[random2].nome;
                    this.reservas[random2].nome = swap;
                    this.divisoes[3].participantes.find(x => x.posicao == 7).rotulo = "subiu"
                }
               }
               for(let d of this.divisoes){
                   for(let p of d.participantes){
                       p.posicao = 0;
                   }
               }
           },

           processarNovaDivisao(){
                this.mostrarBotaoProximaDivisao = false;
                if(this.divisaoAtual == 5){
                   this.processarDivisoesProximaTemporada();
                   this.divisaoAtual = 1;
                }
                
                for(let i = 0; i < this.participantes.length; i++){
                    Vue.set(this.participantes, i, {"id": i + 1, "nome": this.divisoes[this.divisaoAtual - 1].participantes[i].nome, "pontuacao": [], "total": 0, "pontuacaoFora": 0, "acumulado": ["", "", "", "", "", "", "", "", "", "", "", "", "", ""]})
                }
                for(let i = 0; i < 14; i++){
                    this.$refs["rodada" + (1 + i)].zerarInputs();
                }
                this.rodadaAtual = 1;
                this.partidaAtual =  1;
                this.jogadorAtual = 1;
           },

           toggleEditarParticipantes(){
               if(this.editarParticipantes){
                   localStorage.divisoes = JSON.stringify(this.divisoes);
                   localStorage.reservas = JSON.stringify(this.reservas);
                   localStorage.divisaoAtual = this.divisaoAtual;
               }
               this.editarParticipantes = !this.editarParticipantes;
           },

           removeReserva(){
               this.reservas.pop();
           },

           addReserva(){
               this.reservas.push({"nome": ""});
           }
        },
        created: function(){
            for(let i = 0; i < this.participantes.length; i++){
                this.participantes[i].nome = this.divisoes[this.divisaoAtual - 1].participantes[i].nome;
            }            
        },
        mounted() {
            if (localStorage.divisaoAtual) {
                this.divisaoAtual = parseInt(localStorage.divisaoAtual);
                this.divisoes = JSON.parse(localStorage.divisoes);
                this.reservas = JSON.parse(localStorage.reservas);
                console.log(this.divisoes);
                this.processarNovaDivisao();
            }
            this.iniciado = true;
        },
        watch: {
            divisaoAtual(value) {
                if(this.iniciado && value <= 4){
                    localStorage.divisaoAtual = this.divisaoAtual;
                    localStorage.divisoes = JSON.stringify(this.divisoes);
                    localStorage.reservas = JSON.stringify(this.reservas);
                }

            }
        }
    }
</script>
