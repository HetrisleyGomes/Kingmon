<template>
<div>
    <div class="d-flex justify-content-center">
    <table class="table w-25 t1 border m-2">
        <thead class="table align-middle"><td colspan="3">Modo Campanha</td></thead>
        <tr>
        <th style="width: 40%;">Fase</th>
        <td class="corClara">{{ fase }}</td>
        <td style="width:20%;"><button class="btn btn-warning" @click="fase0()" v-if="fase > 1">Recomeçar</button></td>
        </tr>
        <tr>
        <th>Vitorias</th>
        <td class="corClara">{{ vitorias }} <ion-icon name="trophy" style="margin-bottom: -10px;"></ion-icon></td>
        <td><button class="btn btn-danger" @click="vitoria0()">Reiniciar</button></td>
        </tr>
    </table>
    <table class="table w-25 t1 border m-2">
        <thead class="table align-middle"><td colspan="3">Modo Desafio</td></thead>
        <tr>
            <th style="width: 40%;">Fase atual</th>
            <td class="corClara">{{ d_fase_atual }}</td>
            <td style="width:20%;"><button class="btn btn-warning" @click="fase_desafio0()" v-if="d_fase_atual > 1">Recomeçar</button></td>
        </tr>
        <tr>
            <th>Fase maxima</th>
            <td class="corClara">{{ d_fase_maxima }}</td>
            <td><button class="btn btn-danger" @click="max_desafio0()" v-if="d_fase_maxima > 0">Reiniciar</button></td>
        </tr>
        <tr>
            <th>Vitorias</th>
            <td class="corClara">{{ d_vitorias }} <ion-icon name="trophy" style="margin-bottom: -10px;"></ion-icon> </td>
            <td><button class="btn btn-danger" @click="vitoria_desafio0()">Reiniciar</button></td>
        </tr>
    </table>
    </div>
    <p>Hetrisley &copy; 2022 <br>V:{{ logs[0].version }} </p>
    <div class="d-flex justify-content-center row p-3 " v-if="vitorias > 0">
    <h2 class="">Bônus de vitórias <button @click="hidde_bv = true" v-if="hidde_bv == false"><ion-icon name="chevron-down" style="margin-bottom: -5px;"></ion-icon></button><button @click="hidde_bv = false" v-if="hidde_bv == true"><ion-icon name="chevron-up" style="margin-bottom: -5px;"></ion-icon></button></h2>
        <div v-if="hidde_bv == true">
        <div class="fieldset" style="margin-bottom: 10px">
            <legend>Personagens (vitórias 1)</legend>
            <p>Liberado Três novos personagens: Hisuian Rowlet, Hisuian Cindaquil e Hisuian Oshawott</p>
        </div> 
        <div v-if="vitorias >= 2" class="fieldset" style="margin-bottom: 10px">
            <legend>Sincronias (vitórias 2)</legend>
            <div>
                <p><strong>Sincronia de Grama:</strong> +1 de ataque para todos os pokemons. +1 de regeneração.</p>
                <p><strong>Sincronia de Fogo:</strong> +1 de ataque para todos os pokemons. +1 de velocidade para todos os pokemons.</p>
                <p><strong>Sincronia de Água:</strong> +1 de defesa para todos os pokemons. +1 de Vida máxima.</p>
                <p><strong>Sincronia Tripla:</strong> +1 de ataque para todos os pokemons. +1 de defesa para todos os pokemons.</p>
            </div>
            <hr>
            <div>
                <p><strong>Sincronia Atacante:</strong> +1 de ataque para todos os pokemons. +1 de Vida máxima.</p>
                <p><strong>Sincronia Defensiva:</strong> +1 de defesa para todos os pokemons. +1 de velocidade para todos os pokemons.</p>
                <p><strong>Sincronia Auxiliar:</strong> +1 de regeneração. +1 de Vida máxima. </p>
                <p><strong>Sincronia de Equipe:</strong> +1 de defesa para o pokemon na linha de frente. +1 de ataque para o pokemon no meio. +1 de velocidade para o pokemon da linha de trás. +1 de regeneração.</p>
            </div>
        </div>
        <div v-if="vitorias >= 3" class="fieldset" style="margin-bottom: 10px">
            <legend>Habilidades (vitórias 3)</legend>
            <div>
                <p><strong>Bônus Ágil:</strong> Inflige um dano um pouco mais frágil antes do combate começar. </p>
                <p><strong>Bônus Eclético:</strong> Tem uma chance de esquivar do ataque do oponente. </p>
                <p><strong>Bônus Estrategista:</strong> Ganha um pequeno bônus de ataque e defesa. </p>
                <p><strong>Bônus Range:</strong> Quando atacado, inflige metade do dano ao oponente. </p>
                <p><strong>Bônus Bruto:</strong> Ao diminuir a velocidade, aumenta o seu poder de ataque. </p>
            </div>
            <legend>Personagens</legend>
            <p>Liberado Três novos personagens: Pansage, Pansear e Panpour</p>
        </div> 
        <div v-if="vitorias >= 6" class="fieldset" style="margin-bottom: 10px">
            <legend>Personagens (vitórias 6)</legend>
            <p>Liberado Quatro novos personagens: Pikachu, Eevee, Absol e Poipole</p>
        </div> 
        <div v-if="vitorias >= 7" class="fieldset" style="margin-bottom: 10px">
            <legend>Sincronias (vitórias 7)</legend>
            <p><strong>Protagonista</strong> +1 de ataque para todos os pokemons. +1 de regeneração.</p>
            <p><strong>Protagonista - II</strong> +2 de ataque para todos os pokemons. +1.5 de regeneração.</p>
            <legend>Habilidades (vitórias 7)</legend>
            <p><strong>Bônus Assassino:</strong> Impede que o oponente se regenere, além de curar 1 PV a cada turno.</p>
        </div> 
        <div v-if="vitorias >= 8" class="fieldset" style="margin-bottom: 10px">
            <legend>Sincronias (vitórias 8)</legend>
            <p><strong>Sincronia Elétrica:</strong> [+2 Água] +1 de defesa para todos os pokemons. +1 de Vida máxima.</p>
            <p><strong>Sincronia Normal:</strong> [+2 Pokemon] +1.5 de defesa para todos os pokemons. +0.5 de Regeneração.</p>
            <p><strong>Sincronia Sombria:</strong> [+2 Fogo] +2 de velocidade para todos os pokemons.</p>
            <p><strong>Sincronia Veneno:</strong> [+2 Grama] +1.5 de ataque para todos os pokemons. +1.5 de Regeneração.</p>
            <hr>
            <p><strong>Sincronia do Héroi</strong> 1 pokemon Protagonista + 1 Pokemon Tank + 1 Pokemon Suporte. +1 de ataque e defesa para todos os pokemons. +1.5 de regeneração.</p>
        </div> 
            <span v-if="vitorias == 8">Não há mais coisas a serem descobertas :)</span>
        </div>
    </div>
    <div class="d-flex justify-content-center row p-3">
        <h3>Lista de atualizações <button @click="hidde_logs = true" v-if="hidde_logs == false"><ion-icon name="chevron-down" style="margin-bottom: -5px;"></ion-icon></button><button @click="hidde_logs = false" v-if="hidde_logs == true"><ion-icon name="chevron-up" style="margin-bottom: -5px;"></ion-icon></button></h3>
        <div v-if="hidde_logs == true">
            <p v-for="log in logs" :key="log.version"><span class="textao"><strong>Versão: {{log.version}}</strong> - data: {{ log.data }}</span><ul class="list-group" style="width: 50%; left:auto; right: auto; margin:auto"><li v-for="f in log.features" :key="f.index" class="list-group-item">{{f}}</li></ul></p>
        </div>
    </div>
    <footer class="footer">
    <div class="">
        <p>Hetrisley &copy; 2022 <br>V:{{ logs[0].version }} </p>
    </div>
    </footer>
</div>
</template>

<script>
    export default {
     name: 'sobre',
     data() {return{
      page: 0,
      fase: 1,
      vitorias: 0,
      d_fase_atual: 1,
      d_fase_maxima: 0,
      d_vitorias: 0,
      d_page: 0,
      hidde_bv: false,
      hidde_logs: false,
      logs:[
            {"version":"0.0.7",
             "data":"03/11/2022",
             "features":["Melhorias do sistema"]},
            {"version":"0.0.6",
             "data":"28/10/2022",
             "features":["Nova aba 'Pokemons'","1 Novo personagem jogável (Poipole)","Tipo 'Extra' removido","4 Novos tipos [Elétrico, Normal, Sombrio, Veneno](ainda não implementado os bônus)"]},
            {"version":"0.0.5",
             "data":"22/10/2022",
             "features":["3 Novos personagens Jogáveis [Pansage, Pansear, Panpour]","Novo Tipo de dano: 'Prankster' (ainda não implementado os bônus)","Melhorias na gameplay","Correções de bugs", "Modo Desafio com 50 níveis"]},
             {"version":"0.0.4",
             "data":"10/09/2022",
             "features":["3 Novos personagens Jogáveis [Pikachu, Eevee, Absol]","Novo Tipo 'Extra'","Nova Classe 'Protagonista'","Novo Tipo de dano: 'Assassino'"]},
             {"version":"0.0.3",
             "data":"09/09/2022",
             "features":["Correções de bugs", "Nova Aba 'Sobre' adicionada", "Informações extras adicionadas", "Testes adicionais"]},
             {"version":"0.0.2",
             "data":"08/09/2022",
             "features":["Correções de bugs", "3 Novos personagens jogáveis [Iniciais de Hisui]"]},
             {"version":"0.0.1",
             "data":"06/09/2022",
             "features":["Lançamento oficial da aplicação.","24 Personagens jogáveis","Modo Campanha com 15 níveis"]},
            ]
  
      }
    }, methods: {
          async getBonus(){
            const req = await fetch('http://localhost:3000/bonus');
            const data = await req.json();
  
            this.page = data[1].extra.page
            this.fase = data[1].extra.fase
            this.vitorias = data[1].extra.vitorias

            this.d_fase_atual = data[2].desafio.fase_atual
            this.d_fase_maxima = data[2].desafio.fase_maxima
            this.d_vitorias = data[2].desafio.vitorias
            this.d_page = data[2].desafio.page
        },
        fase0(){
            if (window.confirm('Quer recomeçar a campanha?')){
                setTimeout(this.setChars(),400);
                this.setFase0();
                this.$emit('desbloquear')
                setTimeout(() => {
                    this.getBonus();
                }, 800);
            }
        },
        vitoria0(){
            if (window.confirm('Quer recomeçar a campanha?')){
                setTimeout(this.setChars(),400);
                this.setVitoria0();
                setTimeout(() => {
                    this.getBonus();
                }, 800);
            }
        },
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
        async setFase0(){
            var data = {"extra": {
                        "page": 0,
                        "fase": 1,
                        "vitorias": this.vitorias
                      }
                      }
            const datajson = JSON.stringify(data);

            await fetch(`http://localhost:3000/bonus/1`,{
                method: "PUT",
                headers: {"content-type":"application/json"},
                body: datajson
            })
        },
        async setVitoria0(){
            var data = {"extra": {
                        "page": 0,
                        "fase": 1,
                        "vitorias": 0
                      }
                      }
            const datajson = JSON.stringify(data);

            await fetch(`http://localhost:3000/bonus/1`,{
                method: "PUT",
                headers: {"content-type":"application/json"},
                body: datajson
            })
        },
        // DESAFIO
        fase_desafio0(){
            if (window.confirm('Quer recomeçar o desafio?')){
                setTimeout(this.setChars(),400);
                this.setFaseDesafio0();
                this.$emit('desbloquear')
                setTimeout(() => {
                    this.getBonus();
                }, 800);
            }
        },
        max_desafio0(){
            if (window.confirm('Quer recomeçar limpar o máximo do desafio?')){
                setTimeout(this.setChars(),400);
                this.setMaximo0();
                setTimeout(() => {
                    this.getBonus();
                }, 800);
            }
        },
        vitoria_desafio0(){
            if (window.confirm('Quer recomeçar o desafio?')){
                setTimeout(this.setChars(),400);
                this.setVitoriaDesafio0();
                setTimeout(() => {
                    this.getBonus();
                }, 800);
            }
        },
        async setFaseDesafio0(){
            var data = {"desafio": {
                        "fase_atual": 1,
                        "fase_maxima": this.d_fase_maxima,
                        "vitorias": this.d_vitorias,
                        "page": 0
                        }
                      }
            const datajson = JSON.stringify(data);

            await fetch(`http://localhost:3000/bonus/2`,{
                method: "PUT",
                headers: {"content-type":"application/json"},
                body: datajson
            })

        },
        async setMaximo0(){
            var data = {"desafio": {
                        "fase_atual": 1,
                        "fase_maxima": 0,
                        "vitorias": this.d_vitorias,
                        "page": 0
                        }
                      }
            const datajson = JSON.stringify(data);

            const req = await fetch(`http://localhost:3000/bonus/2`,{
                method: "PUT",
                headers: {"content-type":"application/json"},
                body: datajson
            })

            const res = await req.json();
            data = res
        },
        async setVitoriaDesafio0(){
            var data = {"desafio": {
                        "fase_atual": 1,
                        "fase_maxima": 0,
                        "vitorias": 0,
                        "page": 0
                        }
                      }
            const datajson = JSON.stringify(data);

            const req = await fetch(`http://localhost:3000/bonus/2`,{
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