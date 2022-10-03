<template>
<div>
    <div class="d-flex justify-content-center">
    <table class="table w-25 t1">
        <thead class="table align-middle"><td colspan="3">Modo Campanha</td></thead>
        <tr>
        <th style="width: 40%;">Fase</th>
        <td class="corClara">{{ fase }}</td>
        <td style="width:20%;"><button class="btn btn-warning" @click="fase0()" v-if="fase > 1">Recomeçar</button></td>
        </tr>
        <tr>
        <th>Vitorias</th>
        <td class="corClara">{{ vitorias }}</td>
        <td><button class="btn btn-danger" @click="vitoria0()">Reiniciar</button></td>
        </tr>
    </table>
    </div>
    
    <div class="d-flex justify-content-center row p-3 " v-if="vitorias > 0">
    <h2 class="">Bônus de vitórias</h2>
        <div class="fieldset" style="margin-bottom: 10px">
            <legend>Personagens</legend>
            <p>Liberado Três novos personagens: Hisuian Rowlet, Hisuian Cindaquil e Hisuian Oshawott</p>
        </div> 
        <div v-if="vitorias >= 2" class="fieldset" style="margin-bottom: 10px">
            <legend>Sincronias</legend>
            <div>
                <p><strong>Sincronia de Grama:</strong> +1 de ataque para todos os pokemons. +1 de regeneração.</p>
                <p><strong>Sincronia de Fogo:</strong> +1 de ataque para todos os pokemons. +1 de velocidade para todos os pokemons.</p>
                <p><strong>Sincronia de Água:</strong> +1 de defesa para todos os pokemons. +1 de Vida máxima.</p>
                <p><strong>Sincronia Tripla:</strong> +1 de ataque para todos os pokemons. +1 de defesa para todos os pokemons.</p>
                <p v-if="vitorias >= 5">Um tipo extra é valido para fazer sincronia de um mesmo tipo.</p>
            </div>
            <hr>
            <div>
                <p><strong>Sincronia Atacante:</strong> +1 de ataque para todos os pokemons. +1 de Vida máxima.</p>
                <p><strong>Sincronia Defensiva:</strong> +1 de defesa para todos os pokemons. +1 de velocidade para todos os pokemons.</p>
                <p><strong>Sincronia Auxiliar:</strong> +1 de regeneração. +1 de Vida máxima. </p>
                <p><strong>Sincronia de Equipe:</strong> +1 de defesa para o pokemon na linha de frente. +1 de ataque para o pokemon no meio. +1 de velocidade para o pokemon da linha de trás. +1 de regeneração.</p>
                <p v-if="vitorias >= 5"><strong>Protagonista</strong> dá bônus de Atacante e Suporte. Ele tem sincroia propria dependendo da posição: +1 de defesa e velocidade na linha de frente. +2 de defesa no meio. +1 de defesa e ataque na linha de trás. Além de +0.5 de regeneração.</p>
                <p v-if="vitorias >= 5"><strong>Sincronia do Héroi</strong> +1 de ataque para todos os pokemons. +1 de defesa para todos os pokemons. +1 de defesa adicional para o pokemon protagonista. +1 de regeneração. </p>
                <p v-if="vitorias >= 7"><strong>Sincronia do Héroi</strong> 1 pokemon Protagonista. 1 Pokemon Tank. 1 Pokemon Suporte. </p>
            </div>
            <hr>
            <div v-if="vitorias >= 3">
                <p><strong>Bônus Ágil:</strong> Inflige um dano um pouco mais frágil antes do combate começar. </p>
                <p><strong>Bônus Eclético:</strong> Tem uma chance de esquivar do ataque do oponente. </p>
                <p><strong>Bônus Estrategista:</strong> Ganha um pequeno bônus de ataque e defesa. </p>
                <p><strong>Bônus Range:</strong> Quando atacado, inflige metade do dano ao oponente. </p>
                <p><strong>Bônus Bruto:</strong> Ao diminuir a velocidade, aumenta o seu poder de ataque. </p>
                <p v-if="vitorias >= 5"><strong>Bônus Assassino:</strong> Impede que o oponente se regenere, além de curar 1 PV a cada turno.. </p>
            </div>
        </div>
        <div v-if="vitorias >= 5" class="fieldset" style="margin-bottom: 10px">
            <legend>Personagens</legend>
            <p>Liberado Três novos personagens: Pikachu, Eevee e Absol</p>
        </div> 
            <span v-if="vitorias == 5">Não há mais coisas a serem descobertas :)</span>
    </div>
    
    <div class="d-flex justify-content-center row p-3">
        <h3>Lista de atualizações:</h3>
        <p v-for="log in logs" :key="log.version"><span class="textao"><strong>Versão: {{log.version}}</strong> - data: {{ log.data }}</span><ul class="list-group" style="width: 30%; left:auto; right: auto; margin:auto"><li v-for="f in log.features" :key="f.index" class="list-group-item">{{f}}</li></ul></p>
    </div>
    <footer class="footer">
    <div class="">
        <p>Hetrisley &copy; 2022 <br> V: 0.0.4</p>
    </div>
    </footer>
</div>
</template>

<script>
    export default {
     name: 'sobre',
     data() {return{
      vida_atual: 0,
      vida_max: 0,
      reg: 0,
      page: 0,
      fase: 1,
      vitorias: 0,
      logs:[
             {"version":"0.0.4",
             "data":"10/09/2022",
             "features":["3 Novos personagens Jogáveis","Novo Tipo 'Extra'","Nova Classe 'Protagonista'","Novo Tipo de dano: 'Assassino'"]},
             {"version":"0.0.3",
             "data":"09/09/2022",
             "features":["Correções de bugs", "Nova Aba 'Sobre' adicionada", "Informações extras adicionadas","Testes adicionais"]},
             {"version":"0.0.2",
             "data":"08/09/2022",
             "features":["Correções de bugs", "3 Novos personagens jogáveis"]},
             {"version":"0.0.1",
             "data":"06/09/2022",
             "features":["Lançamento oficial da aplicação.","24 Personagens jogáveis","Modo Campanha com 15 níveis"]},
            ]
  
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
        },
        fase0(){
            if (window.confirm('Quer recomeçar a campanha?')){
                this.setChars();
                this.setFase0();
                setTimeout(() => {
                    this.getBonus();
                }, 800);
            }
        },
        vitoria0(){
            if (window.confirm('Quer recomeçar a campanha?')){
                this.setChars();
                this.setVitoria0();
                setTimeout(() => {
                    this.getBonus();
                }, 800);
            }
        },
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
        },
        async setFase0(){
            var data = {"extra": {
                        "vida_atual": 10,
                        "vida_max":  10,
                        "reg": 0,
                        "page": 0,
                        "fase": 1,
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
        async setVitoria0(){
            var data = {"extra": {
                        "vida_atual": 10,
                        "vida_max":  10,
                        "reg": 0,
                        "page": 0,
                        "fase": 1,
                        "vitorias": 0
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
        }
      }, mounted() {
        this.getBonus();
      }
    }
</script>

<style scoped>

table.t1, .textao{
    font-size: 20px;
}

.fieldset {
    border: solid 1px #2c3e50;
    width: 70%; 
    left:auto; 
    right: auto; 
    margin:auto;
}

p {
    font-size: 16px
}

.corClara{
    color:#7d8d9e;
}

footer{
    border-top: 1px solid #888;
    padding: 20px;
    text-align: center;
    margin-bottom: 0;
}

button {
    width: auto;
    color: black;
    border: none;
    padding: 5px 10px;
    margin: 2px;
    border-radius: 40px;
    text-align: center;
}
</style>