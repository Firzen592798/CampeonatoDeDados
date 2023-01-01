<template>
  <div id="app">
    <!--<Agenda></Agenda>-->
    <Tabela></Tabela>
  </div>
</template>

<script>

//import Agenda from "./components/Agenda"
import Tabela from "./components/Tabela"

export default {
  name: 'app',
  components: {
    Tabela
  }
}
</script>

<style>
@import "~bulma/css/bulma.css";
@import "~font-awesome/css/font-awesome.css";
#app {

}
</style>
