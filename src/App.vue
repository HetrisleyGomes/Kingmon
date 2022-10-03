<template>
  <div id="nav">
    <router-link v-if="!block_campanha" to="/">Campanha</router-link><span v-if="!block_campanha"> | </span>
    <router-link v-if="!block_desafio" to="/desafio">Desafio</router-link><span v-if="!block_desafio"> | </span>
    <router-link to="/about">Sobre</router-link> |
    <router-link to="/dashboard">{Time}</router-link> |
  </div>
  <router-view @bloquear_desafio="bloquear_desafio" @bloquear_campanha="bloquear_campanha" @desbloquear="desbloquear" />
</template>

<script>
  export default {
    data(){return{
      block_campanha: false,
      block_desafio: false,
    }}, methods:{
      bloquear_desafio(){
        this.block_desafio = true
      },
      bloquear_campanha(){
        this.block_campanha = true
      },
      desbloquear(){
        this.block_campanha = false
        this.block_desafio = false
      }
    }
  }
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 10px 20px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}

.amarelo{
  color:#d1df0c; text-decoration: solid;
}
</style>
