<template>
    <div class="home">
      <div v-if="alfa">
        Esse modo de jogo ainda está em desenvolvimento :) <br>
        <input type="button" @click="blockotherpages()" class="btn btn-success" value="Iniciar desafio">
      </div>

      <div>
        <Preparar :vitorias="5" />
      </div>
    </div>
  </template>
  
<script>
import Preparar from '../components/desafio_components/Preparar.vue';
export default {
  name: 'Desafio',
  components: {
    Preparar
  }, data() {return{
    
      alfa:false
    }
  }, methods: {
    blockotherpages(){
      //this.$emit('bloquear_campanha');
    }
  }, mounted() {

  },
}
</script>
  