# Campeonato de dados


![vue.js](https://img.shields.io/badge/Vue.js-35495E?style=for-the-badge&logo=vue.js&logoColor=4FC08D)

Projeto front end feito em Vue.js com um intuito de rolar um campeoanto de 8 participantes usando um dado de 6 faces para representar os jogos. Projeto feito exclusivamente para praticar aspectos de reatividade e composição na tecnologia vue.js enquanto coloca em prática um antigo hobby. O projeto funciona numa tela de dispositivo móvel <br/>

[Link para o projeto em execução](https://campeonatodedados.web.app/)

<div style="display: flex; justify-content:space-between;">

<img width=780 src="https://user-images.githubusercontent.com/1834518/210160151-3b23c789-e15a-4a37-8eb9-78bf643e2947.png"/>

<img width=210 src="https://user-images.githubusercontent.com/1834518/210160377-0d2ed418-22ff-4075-bc9f-a2f3fbea604e.png"/>

</div>

## Características
<ul>
<li>O sistema possui um sistema de 4 divisões com 8 participantes em que todos enfrentam todos, ida e volta. Os dois primeiros sobem e os dois últimos são rebaixados.
<li>Após acabar a primeira divisão, o jogo vai para a segunda divisão, depois para a terceira e quarta divisão e volta para a primeira divisão e assim sucessivamente</li>
<li>Os rebaixados da quarta divisão podem ser substituídos por reservas. </li>
<li>O navegador salva as informações do campeonato após a conclusão do campeonato de uma divisão </li>
<li>O sistema fornece um ponto a mais no dado para quem está "jogando em casa". Caso os dois jogadores tirem 1 ou 6 no dado, o sistema considera empate, foi a forma encontrada para evitar vitória garantida ao tirar 6 em casa ou derrota ao tirar 1 fora de casa, além de aumentar o número de empates</li>
<li>É possível editar os participantes e colocar os nomes que quiser</li>

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Deploy
```
firebase deploy --only hosting
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
