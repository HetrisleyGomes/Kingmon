<template>
    <div class="home">
      <div v-if="page == 0">
        <p style="margin: 10px;">Prepare sua equipe e enfrente um modo sem brutal sem limites!</p>
        <input type="button" @click="blockotherpages()" class="btn btn-success" value="Iniciar desafio">
      </div>
      <div v-else-if="page == 1">
        <Preparar :vitorias="vitorias" @page2="Page2" />
      </div>
      <div v-else-if="page == 2">
        <Battle :fase="fase_atual" @page2="Page2" @fase1="Fase1" @lose="Derrota" @vitoria="vitoria" />
      </div>
    </div>
  </template>
  
<script>
import Preparar from '../components/desafio_components/Preparar.vue';
import Battle from '../components/desafio_components/Battle.vue';
export default {
  name: 'Desafio',
  components: {
    Preparar,
    Battle
  }, data() {return{
      fase_atual: 1,
      fase_maxima: 0,
      vitorias: 0,
      vitorias_desafio: 0,
      page: 0
    }
  }, methods: {
    blockotherpages(){
      this.page = 1
      this.$emit('bloquear_desafio');
    },
    Page2(){
      this.page = 2
      this.setPage()
    },
    Fase1(){
      this.fase_atual ++
      this.setPage()
    },
    async getBonus(){
      const req = await fetch('http://localhost:3000/bonus');
      const data = await req.json();

      this.vida_atual = data[2].desafio.vida_atual
      this.vida_max = data[2].desafio.vida_max
      this.fase_atual = data[2].desafio.fase_atual
      this.fase_maxima = data[2].desafio.fase_maxima
      this.vitorias = data[1].extra.vitorias
      this.vitorias_desafio = data[2].desafio.vitorias
      this.page = data[2].desafio.page
    },
    async setPage(){
        var data = {"desafio": {
                        "fase_atual" : this.fase_atual,
                        "fase_maxima" : this.fase_maxima,
                        "vitorias" : this.vitorias_desafio,
                        "page" : this.page
                      }
                      }
        const datajson = JSON.stringify(data);

        await fetch(`http://localhost:3000/bonus/2`,{
            method: "PUT",
            headers: {"content-type":"application/json"},
            body: datajson
        })
    },
    /*Caso perda você volta do inicio, isso aqui apaga o seu time*/
      async Derrota(){
        this.fase_maxima = this.fase_atual > this.fase_maxima? this.fase_atual: this.fase_maxima
        this.fase_atual = 1
        this.page = 0
        setTimeout(this.setPage(),500)
        const p = {
                "p": {
                  "nome":"",
                  "img":"placeholder",
                  "tipo":"",
                  "desc":"",
                  "damage_type": "",
                  "class":"",
                  "atk":0,
                  "def":0,
                  "spd":0
                }
              }
        this.$emit('desbloquear');
        const datajson = JSON.stringify(p);

        setTimeout(await fetch(`http://localhost:3000/time/1`,{
            method: "PUT",
            headers: {"content-type":"application/json"},
            body: datajson
        }),300)
        setTimeout(await fetch(`http://localhost:3000/time/2`,{
            method: "PUT",
            headers: {"content-type":"application/json"},
            body: datajson
        }),600)
        setTimeout(await fetch(`http://localhost:3000/time/3`,{
            method: "PUT",
            headers: {"content-type":"application/json"},
            body: datajson
        }),900)


      },
      async vitoria(){
        this.vitorias_desafio++
        this.Derrota()
      },
  }, mounted() {
    this.getBonus()
  },
}
</script>
  