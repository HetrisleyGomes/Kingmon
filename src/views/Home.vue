<template>
  <div class="home">
    <!--<input type="button" value="-" @click="page--">{{ page }}<input type="button" value="+" @click="page++">-->
    <div v-if="dev_mode">
      <input type="button" class="btn btn-info" value="Continuar" @click="setChars()" style="color:#fff">
    </div>
    <div v-if="page == 0">
      <Listar @page1='Page1' :vitorias="vitorias" />
    </div>
    <div v-else-if="page==1">
      <Battle :fase="fase" @page0='Page2' @lose="setChars" @page2='Page2' @fase1='fase1' @vitoria="vitoria" :vitorias="vitorias" />
    </div>
    <div v-else-if="page==2">
      <Lojinha @page3='Page3' :vitorias="vitorias" />
    </div>
    <div v-else-if="page==3">
      <Team @page1='Page1' />
    </div>
    <div v-else-if="page==4">
      <input type="button" @click="blockotherpages()" class="btn btn-success" value="Iniciar campanha">
      <p style="margin: 10px;">Enfrente 15 fases construindo sua equipe e sua estratégia para vencer esse modo!</p>
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
    vida_atual: 0,
    vida_max: 0,
    reg: 0,
    page: 4,
    fase: 1,
    vitorias: 0,

    dev_mode: false
    }
  }, methods: {
        async getBonus(){
          const req = await fetch('http://localhost:3000/bonus');
          const data = await req.json();

          this.vida_atual = data[3].extra.vida_atual
          this.vida_max = data[3].extra.vida_max
          this.reg = data[3].extra.reg
          this.page = data[3].extra.page
          this.fase = data[3].extra.fase
          this.vitorias = data[3].extra.vitorias
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
      }, fase1(){
        this.fase++
      },
       async setPage(){
        var data = {"extra": {
                        "vida_atual": this.vida_atual,
                        "vida_max":  this.vida_max,
                        "reg": this.reg,
                        "page": this.page,
                        "fase": this.fase,
                        "vitorias": this.vitorias
                      }
                      }
        const datajson = JSON.stringify(data);

        const req = await fetch(`http://localhost:3000/bonus/4`,{
            method: "PUT",
            headers: {"content-type":"application/json"},
            body: datajson
        })

        const res = await req.json();
        data = res
      },
      /*Caso perda você volta do inicio, isso aqui apaga o seu time*/
      async setChars(){
        const p1 = {
                "id": 1,
                "p1": {
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
        const p2 = {
            "id": 2,
            "p2": {
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
        const p3 = {
            "id": 3,
            "p3": {
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

        const datajson1 = JSON.stringify(p1);
        const datajson2 = JSON.stringify(p2);
        const datajson3 = JSON.stringify(p3);
        var req = await fetch(`http://localhost:3000/time/1`,{
            method: "PUT",
            headers: {"content-type":"application/json"},
            body: datajson1
        })
        var req2 = await fetch(`http://localhost:3000/time/2`,{
            method: "PUT",
            headers: {"content-type":"application/json"},
            body: datajson2
        })
        var req3 = await fetch(`http://localhost:3000/time/3`,{
            method: "PUT",
            headers: {"content-type":"application/json"},
            body: datajson3
        })

        const res = await req.json();
        const res2 = await req2.json();
        const res3 = await req3.json();
        if (res != null || res2 != null || res3 != null){
          console.log('ok')
        }
        this.fase = 1
        this.page = 4
        this.setPage();
        this.$emit('desbloquear');
      },
      vitoria(){
        this.vitorias++
        window.confirm('Voce venceu!\nVeja seu time ou jogue de novo!')
        this.setChars()
      }, blockotherpages(){
        this.page = 0; 
        console.log('foi o bloqueio'); 
        this.$emit('bloquear_desafio');
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