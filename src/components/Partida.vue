<template>
    <div class="partida">
        <label>{{p1}}</label>
        <input readonly type="number" v-model="value1"/>
        <span> X </span>
        <input readonly type="number" v-on:keyup="enviarPartida" v-model="value2"/>
        <label style="text-align: left">{{p2}}</label>
    </div>
</template>

<script>

    export default{
        name: 'Partida',
        props: ['p1', 'p2'],
        data (){
            return {
                value1: "",
                value2: "",
                processou: false,
            }
        },
        methods: {
            enviarPartida() {
                if(this.value1 && this.value2 && !this.processou){
                    let resultado = this.calcularVencedorMetodoForaCasa();
                    this.$emit('processarPartida', this.p1, this.p2, resultado);
                    this.processou = true;
                }
            },
            calcularVencedorMetodoNormal(){
                    let resultado = '';
                    if(this.value1 == this.value2){
                        resultado = 'e';
                    }
                    else if(this.value1 > this.value2){
                        resultado = 'v';
                    }
                    else{
                        resultado = 'd';
                    }
                    return resultado;
            },

            calcularVencedorMetodoForaCasa(){
                var resultado = '';
                if((this.value1 == 6 && this.value2 == 6) ||(this.value1 == 1 && this.value2 == 1)){
                    resultado = 'e';
                }else{
                this.value1++;
                if(this.value1 == this.value2){
                        resultado = 'e';
                    }
                    else if(this.value1 > this.value2){
                        resultado = 'v';
                    }
                    else{
                        resultado = 'd';
                    }
                }
                return resultado;
            },
            receberJogada(dado, jogadorAtual){
                if(jogadorAtual == 1){
                    this.value1 = dado;
                }else if(jogadorAtual == 2){
                    this.value2 = dado;
                }
                this.enviarPartida();
            },

            zerarInputs(){
                this.value1 = "";
                this.value2 = "";
                this.processou = false;
            }
        }
    }

</script>
<style scoped>
    input{
        width: 30px;
    }
    label{
        display: inline-block;
        width:  100px;
        text-align: right;
        margin: 2px 5px 5px 2px;
    }
    input::-webkit-outer-spin-button,
    input::-webkit-inner-spin-button {
        -webkit-appearance: none;
        margin: 0;
    }
</style>