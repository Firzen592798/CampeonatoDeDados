<template>
<div class="rodada">
    Rodada {{rodada}}
    <tr>
        <td>
            <Partida ref="partida1" v-bind:p1="participantes[ordem[0]].nome" v-bind:p2="participantes[ordem[1]].nome" @processarPartida="processarPartida"></Partida>
        </td>
    </tr>
    <tr>
        <td>
            <Partida ref="partida2" v-bind:p1="participantes[ordem[2]].nome" v-bind:p2="participantes[ordem[3]].nome" @processarPartida="processarPartida"></Partida>
        </td>
    </tr>
    <tr>
        <td>
            <Partida ref="partida3" v-bind:p1="participantes[ordem[4]].nome" v-bind:p2="participantes[ordem[5]].nome"  @processarPartida="processarPartida"></Partida>
        </td>
    </tr>
    <tr>
        <td>
            <Partida ref="partida4" v-bind:p1="participantes[ordem[6]].nome" v-bind:p2="participantes[ordem[7]].nome"  @processarPartida="processarPartida"></Partida>
        </td>
    </tr>
    <br/>
</div>
</template>

<script>
    import Partida from './Partida';
    import Vue from 'vue'
    export default {
        name: "Rodada",
        components: {
            Partida
        },
        props: [ 'ordem', 'participantes', 'classificacao', 'rodada' ],
        data() {
            return{

            }
        },
        methods: {
            processarPartida(p1, p2, resultado){
                var pontuacaoP1 = 0;
                var pontuacaoP2 = 0;
                switch(resultado){
                    case 'v':{
                        pontuacaoP1 = 3;
                        pontuacaoP2 = 0;
                    }break;
                    case 'e':{
                        pontuacaoP1 = 1;
                        pontuacaoP2 = 1;
                    }break;
                    case 'd':{
                        pontuacaoP1 = 0;
                        pontuacaoP2 = 3;
                    }break;
                }
                let p1Obj = this.participantes.find(x => x.nome == p1);
                let p2Obj = this.participantes.find(x => x.nome == p2);
                p1Obj.pontuacao.push(pontuacaoP1);
                p2Obj.pontuacao.push(pontuacaoP2);
                p2Obj.pontuacaoFora += pontuacaoP2;
                p1Obj.total += pontuacaoP1;
                p2Obj.total += pontuacaoP2;
                Vue.set(p1Obj.acumulado, this.rodada - 1, p1Obj.total);
                Vue.set(p2Obj.acumulado, this.rodada - 1, p2Obj.total);
            },

            enviarDado(dado, partidaAtual, jogadorAtual)    {
                this.$refs["partida"+partidaAtual].receberJogada(dado, jogadorAtual);
            },

            zerarInputs(){
                for(let i = 0; i < 4; i++){
                    this.$refs["partida"+(1 + i)].zerarInputs();
                }
            }
        }
    }
</script>