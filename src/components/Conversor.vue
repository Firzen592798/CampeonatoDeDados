<template>
    <div class="conversor">
        <h2>{{moedaA}} para {{moedaB}}</h2>
        <input type="text" v-model="moedaA_value" v-bind:placeholder="moedaA">
        <input type="button" value="Converter" v-on:click="converter">
        <h2>{{moedaB_value}}</h2>
    </div>
</template>

<script>

export default {
    name: "Conversor",
    props: ["moedaA", "moedaB"],
    data(){
        return{
            moedaA_value: "",
            moedaB_value: "",
        }
    },
    methods:{
        converter(){
            let de_para = this.moedaA + "_" + this.moedaB;
            let url = "http://free.currencyconverterapi.com/api/v5/convert?q="+de_para+"&compact=&apiKey=d401766c4608efe28389"
            fetch(url).then(res => {
                return res.json()
            }).then(json => {
                console.log(json);
                let cotacao = json.results[de_para].val;
                this.moedaB_value = cotacao * parseFloat(this.moedaA_value).toFixed(2);
            });
        }
    }

}
</script>

<style scoped>
    .conversor{
        padding: 20px;
        max-width: 300px;
        box-shadow: 0 4px 8px 0 black;
    }
</style>