<template>
  <div class="home">
    <!--<input type="button" value="-" @click="page--">{{ page }}<input type="button" value="+" @click="page++">-->
    <div v-if="page==0">
      <p style="margin: 10px;">Enfrente 15 fases construindo sua equipe e sua estratégia para vencer esse modo!</p>
      <input type="button" @click="blockotherpages()" class="btn btn-success" value="Iniciar campanha">
    </div>
    <div v-else-if="page == 1">
      <Listar @page1='Page2' :vitorias="vitorias" />
    </div>
    <div v-else-if="page==2">
      <Battle :fase="fase" @page0='Page1' @lose="setChars" @page2='Page3' @fase1='fase1' @vitoria="vitoria" :vitorias="vitorias" />
    </div>
    <div v-else-if="page==3">
      <Lojinha @page3='Page4' :vitorias="vitorias" />
    </div>
    <div v-else-if="page==4">
      <Team @page1='Page2' />
    </div>

  </div>
</template>

<script>
// @ is an alias to /src
import Listar from '../components/campanha/Listar.vue';
import Battle from '../components/campanha/Battle.vue';
import Lojinha from '../components/campanha/Lojinha.vue';
import Team from '../components/campanha/Team.vue';

export default {
  name: 'Home',
  components: {
    Listar,
    Battle,
    Lojinha,
    Team,
  }, data() {return{
    page: 0,
    fase: 1,
    vitorias: 0,
    }
  }, methods: {
      async getBonus(){
        const req = await fetch('http://localhost:3000/bonus/1');
        const data = await req.json();

        this.page = data.extra.page
        this.fase = data.extra.fase
        this.vitorias = data.extra.vitorias
      }, Page1(){
        this.page = 1
        this.setPage()  
      }, Page0(){
        this.page = 0
        this.setPage() 
      }, Page2(){
        this.page = 2
        this.setPage() 
      }, Page3(){
        this.page = 3
        this.setPage() 
      }, Page4(){
        this.page = 4
        this.setPage() 
      }, fase1(){
        this.fase++
      },
      async setPage(){
        var data = {"extra": {
                        "page": this.page,
                        "fase": this.fase,
                        "vitorias": this.vitorias
                      }
                      }
        const datajson = JSON.stringify(data);

        const req = await fetch(`http://localhost:3000/bonus/1`,{
            method: "PUT",
            headers: {"content-type":"application/json"},
            body: datajson
        })

        const res = await req.json();
        data = res
      },
      /*Caso perda você volta do inicio, isso aqui apaga o seu time*/
      async setChars(){
        this.fase = 1
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
      vitoria(){
        this.vitorias++
        window.confirm('Voce venceu!\nVeja seu time ou jogue de novo!')
        this.setChars()
      }, blockotherpages(){
        this.page = 1; 
        this.$emit('bloquear_campanha');
      }
  }, mounted() {
    this.getBonus();
    if (this.fase == 15){
      setTimeout(() => {
        this.vitoria()
      }, 900);
    }
    
  },
}
</script>