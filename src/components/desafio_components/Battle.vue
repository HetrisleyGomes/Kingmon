<template>
    <div style="overflow-x:hidden; margin-top: -20px;">
        <div class="d-flex justify-content-center row p-3 ms-3 me-3">
            <div class="col" style="width: 90%; left:auto; right: auto; margin:auto">
                <strong class="m-3 mt-1 mb-1">Vida do Time:</strong>
                <div class="row">
                    <div class="progress" :class="[{'col-11': reg != 0}]">
                        <div class="progress-bar" role="progressbar" :style="{ width: (vida_atual/vida_max)*100 > 0 ? (vida_atual/vida_max)*100 + '%' : 0 }" style="border-radius:10px;"></div>
                    </div>
                    <div class="col-1"><span v-if="reg != 0">+{{reg}}</span></div>
                </div>
            </div>
            <div style="display:flex; width: 50px; text-align: left; left:auto; right: auto; margin:auto; margin-bottom:-10px;"><h4>{{fase}}</h4></div>
            <div class="col" style="width: 90%; left:auto; right: auto; margin:auto">
                <strong class="m-3 mt-1 mb-1">Vida do Oponente:</strong>
                <div class="row">
                    <div class="progress" :class="[{'col-11': op_reg != 0}]">
                        <div class="progress-bar" role="progressbar" :class="[{'roxo': tipo_veneno > 0}]" :style="{ width: (op_vida_atual/op_vida_max)*100 > 0 ? (op_vida_atual/op_vida_max)*100 + '%' : 0 }" style="border-radius:10px;"></div>
                    </div>
                    <div class="col-1"><span v-if="op_reg > 0">+{{op_reg}}</span></div>
                </div>
            </div>
        </div>
        <div class="d-flex justify-content-center p-3">
            <table class="table w-75">
                <tr>
                    <td>Start Turn</td>
                    <td v-if="dt_agil == 3" id="Agil">Ágil</td>
                    <td v-if="dt_ecletico == 3" id="Ecletico">Eclético</td>
                    <td v-if="dt_range == 3" id="Range">Range</td>
                    <td v-if="dt_estrategista == 3" id="Estrategista">Estrategista</td>
                    <td v-else-if="dt_bruto == 3" id="Bruto">Bruto</td>
                    <td v-else-if="dt_assassino > 0" id="Assassino">Assassino</td>
                    <td v-else-if="dt_prankster == 3" id="Assassino">Prankster</td>
                    <td v-else> Ataque</td>
                    <td>End Turn</td>
                    
                </tr>
            </table>
        </div>
        <!-- Os Poke -->
        <div class="d-flex justify-content-center row">
            <div class="col" style="width:50%;">
                <div class="card position-fixed" style="width: 50%; left:auto; right: auto; margin:auto">
                    <div class="d-flex">
                        <div class="card-body" v-if="p1.img">
                            <img :src="'/icons/' + p1.img" class="card-img-top" v-if="p1.img != 'placeholder'" alt="..." style="width: 200px; left:auto; right: auto; margin:auto">
                            <h5 class="card-title">{{ p1.nome }}</h5>
                        </div>
                        <div class="card-body" v-if="p2.img">
                            <img :src="'/icons/' + p2.img" class="card-img-top" v-if="p2.img != 'placeholder'" alt="..." style="width: 200px; left:auto; right: auto; margin:auto">
                            <h5 class="card-title">{{ p2.nome }}</h5>
                        </div>
                        <div class="card-body" v-if="p3.img">
                            <img :src="'/icons/' + p3.img" class="card-img-top" v-if="p3.img != 'placeholder'" alt="..." style="width: 200px; left:auto; right: auto; margin:auto">
                            <h5 class="card-title">{{ p3.nome }}</h5>
                        </div>
                    </div>
                </div>
                <div class="card position-fixed" style="width: 50%; left:auto; right: auto; margin:auto">
                    <table>
                        <td style="width: 33%;">Ataque {{ atk_max }}</td>
                        <td style="width: 33%;">Defesa {{ def_max }}</td>
                        <td style="width: 33%;">Velocidade {{ spd_max }}</td>
                    </table>
                </div>
            </div>
            <div class="col" style="width:50%;">
                <div class="card position-fixed" style="width: 50%; left:auto; right: auto; margin:auto">
                    <div class="d-flex">
                        <div class="card-body" v-if="op01.img">
                            <img :src="'/icons/' + op01.img" class="card-img-top" v-if="op01.img != 'placeholder'" alt="..." style="width: 200px; left:auto; right: auto; margin:auto; transform: scaleX(-1); filter: hue-rotate(45deg)">
                            <h5 class="card-title">{{ op01.nome }}</h5>
                        </div>
                        <div class="card-body" v-if="op02.img">
                            <img :src="'/icons/' + op02.img" class="card-img-top" v-if="op02.img != 'placeholder'" alt="..." style="width: 200px; left:auto; right: auto; margin:auto; transform: scaleX(-1); filter: hue-rotate(45deg)">
                            <h5 class="card-title">{{ op02.nome }}</h5>
                        </div>
                        <div class="card-body" v-if="op03.img">
                            <img :src="'/icons/' + op03.img" class="card-img-top" v-if="op03.img != 'placeholder'" alt="..." style="width: 200px; left:auto; right: auto; margin:auto; transform: scaleX(-1); filter: hue-rotate(45deg)">
                            <h5 class="card-title">{{ op03.nome }}</h5>
                        </div>
                    </div>
                </div>
                <div class="card position-fixed" style="width: 50%; left:auto; right: auto; margin:auto">
                    <table>
                        <td style="width: 33%;">Ataque {{ op_atk_max }}</td>
                        <td style="width: 33%;">Defesa {{ op_def_max }}</td>
                        <td style="width: 33%;">Velocidade {{ op_spd_max }}</td>
                    </table>
                </div>
            </div>

        </div>
    </div>
</template>

<script>
export default {
    name: 'Listar',
    data(){
        return{
            vida_max: 10,
            vida_atual: 10,
            reg: 0,
            tipo_grama: 0,
            tipo_fogo: 0,
            tipo_agua: 0,
            tipo_eletrico: 0,
            tipo_normal: 0,
            tipo_sombrio: 0,
            tipo_veneno: 0,
            dt_ecletico: 0,
            dt_agil: 0,
            dt_estrategista: 0,
            dt_bruto: 0,
            dt_range: 0,
            dt_assassino: 0,
            dt_prankster: 0,
            c_atk: 0,
            c_tank: 0,
            c_sup: 0,
            c_pro: 0,
            atk_max: 0,
            def_max: 0,
            spd_max: 0,

            op_vida_max: 10,
            op_vida_atual: 0,
            op_reg: 0,
            op_atk_max: 0,
            op_def_max: 0,
            op_spd_max: 0,
            op01: {
                nome: '',
                img: '',
            },
            op02: {
                nome: '',
                img: '',
            },
            op03: {
                nome: '',
                img: '',
            },

            p1: {
                nome:'',
                img:'',
                tipo:'',
                desc:'',
                damage_type: '',
                classe:'',
                atk:0,
                def:0,
                spd:0
            },
            p2: {
                nome:'',
                img:'',
                tipo:'',
                desc:'',
                damage_type: '',
                classe:'',
                atk:0,
                def:0,
                spd:0
            },
            p3: {
                nome:'',
                img:'',
                tipo:'',
                desc:'',
                damage_type: '',
                classe:'',
                atk:0,
                def:0,
                spd:0
            },
            idmin: 7,
        }
    }, props:{
        fase: Number,
        vitorias: Number,
    },
    methods: {
        async getChars(){
            const req = await fetch('http://localhost:3000/time');
            const data = await req.json();
            this.p1 = data[0].p
            this.p2 = data[1].p
            this.p3 = data[2].p
            this.checarBonus();
        },
        checarBonus(){ 
            this.checkTipo();
            this.checkClass();
            this.checkDT();
            setTimeout(this.calcmax(),200);
        },
        calcmax(){
            let v;
            this.spd_max = this.p1.spd + this.p2.spd + this.p3.spd;
            this.def_max = this.p1.def + this.p2.def + this.p3.def;
            this.spd_max >= 7 ? v = Number((this.spd_max / 7).toFixed(0)) : v = 0
            this.atk_max = this.p1.atk + this.p2.atk + this.p3.atk + v;
            this.vida_atual = this.vida_max;  
        },
        checkTipo(){
            this.tipo_grama = 0
            this.tipo_fogo = 0
            this.tipo_agua = 0
            this.tipo_eletrico = 0
            this.tipo_normal = 0
            this.tipo_sombrio = 0
            this.tipo_veneno = 0
            switch (this.p1.tipo){
                case 'Grama':
                    this.tipo_grama++
                    break;
                case 'Fogo':
                    this.tipo_fogo++
                    break;
                case 'Água':
                    this.tipo_agua++
                    break;
                case 'Elétrico':
                    this.tipo_eletrico++
                    break;
                case 'Normal':
                    this.tipo_normal++
                    break;
                case 'Sombrio':
                    this.tipo_sombrio++
                    break;
                case 'Veneno':
                    this.tipo_veneno++
                    break;
            }
            switch (this.p2.tipo){
                case 'Grama':
                    this.tipo_grama++
                    break;
                case 'Fogo':
                    this.tipo_fogo++
                    break;
                case 'Água':
                    this.tipo_agua++
                    break;
                case 'Elétrico':
                    this.tipo_eletrico++
                    break;
                case 'Normal':
                    this.tipo_normal++
                    break;
                case 'Sombrio':
                    this.tipo_sombrio++
                    break;
                case 'Veneno':
                    this.tipo_veneno++
                    break;
            }
            switch (this.p3.tipo){
                case 'Grama':
                    this.tipo_grama++
                    break;
                case 'Fogo':
                    this.tipo_fogo++
                    break;
                case 'Água':
                    this.tipo_agua++
                    break;
                case 'Elétrico':
                    this.tipo_eletrico++
                    break;
                case 'Normal':
                    this.tipo_normal++
                    break;
                case 'Sombrio':
                    this.tipo_sombrio++
                    break;
                case 'Veneno':
                    this.tipo_veneno++
                    break;
            }

            if (this.tipo_grama == 3) {
                this.p1.atk++
                this.p2.atk++
                this.p3.atk++
                this.reg++
            }
            if (this.tipo_fogo == 3) {
                this.p1.atk++
                this.p2.atk++
                this.p3.atk++
                this.p1.spd++
                this.p2.spd++
                this.p3.spd++
            }
            if (this.tipo_agua == 3) {
                this.p1.def++
                this.p2.def++
                this.p3.def++
                this.vida_max++
            }
            if (this.tipo_grama == 1 && this.tipo_fogo == 1 && this.tipo_agua == 1){
                this.p1.atk++
                this.p2.atk++
                this.p3.atk++
                this.p1.def++
                this.p2.def++
                this.p3.def++
                this.p1.spd++
                this.p2.spd++
                this.p3.spd++
            }
            if (this.tipo_agua == 2 && this.tipo_eletrico == 1) {
                this.p1.def++
                this.p2.def++
                this.p3.def++
                this.p1.spd++
                this.p2.spd++
                this.p3.spd++
                this.vida_max++
            }
            if (this.tipo_normal >= 1 && ((this.tipo_grama + this.tipo_fogo + this.tipo_agua) == 2)){
                this.p1.def+=1.5
                this.p2.def+=1.5
                this.p3.def+=1.5
                this.reg += 0.5
            }
            if (this.tipo_fogo == 2 && this.tipo_sombrio == 1) {
                this.p1.spd+=2
                this.p2.spd+=2
                this.p3.spd+=2
            }
            if (this.tipo_grama == 2 && this.tipo_veneno == 1) {
                this.p1.atk+=1.5
                this.p2.atk+=1.5
                this.p3.atk+=1.5
                this.reg+=1.5
            }
        },
        checkClass(){
            this.c_atk = 0
            this.c_sup = 0
            this.c_tank = 0
            this.c_pro = 0
            switch (this.p1.classe){
                case 'Atacante':
                    this.c_atk++
                    break;
                case 'Protagonista':
                    this.c_pro++
                    break;
                case 'Tank':
                    this.c_tank++
                    break;
                case 'Suporte':
                    this.c_sup++
                    break;
            }
            switch (this.p2.classe){
                case 'Atacante':
                    this.c_atk++
                    break;
                case 'Protagonista':
                    this.c_pro++
                    break;
                case 'Tank':
                    this.c_tank++
                    break;
                case 'Suporte':
                    this.c_sup++
                    break;
            }
            switch (this.p3.classe){
                case 'Atacante':
                    this.c_atk++
                    break;
                case 'Protagonista':
                    this.c_pro++
                    break;
                case 'Tank':
                    this.c_tank++
                    break;
                case 'Suporte':
                    this.c_sup++
                    break;
            }

            if (this.c_atk == 3) {
                this.p1.atk++
                this.p2.atk++
                this.p3.atk++
                this.vida_max++
            }
            if (this.c_tank == 3) {
                this.p1.def++
                this.p2.def++
                this.p3.def++
                this.p1.spd++
                this.p2.spd++
                this.p3.spd++
            }
            if (this.c_sup == 3) {
                this.reg++
                this.vida_max++
            }
            if (this.c_pro == 2){
                this.p1.atk+=2
                this.p2.atk+=2
                this.p3.atk+=2
                this.reg+=1.5
            } else
            if (this.c_pro == 1){
                this.p1.atk++
                this.p2.atk++
                this.p3.atk++
                this.reg++
            } 
            if (this.c_tank == 1 && this.c_sup == 1 && this.c_pro == 1){
                this.p1.atk++
                this.p2.atk++
                this.p3.atk++
                this.p1.def++
                this.p2.def++
                this.p3.def++
                this.reg+=1.5
            }
        },
        checkDT(){
            this.dt_ecletico = 0
            this.dt_agil = 0
            this.dt_estrategista = 0
            this.dt_bruto = 0
            this.dt_range = 0
            this.dt_assassino = 0
            this.dt_prankster = 0
            switch (this.p1.damage_type){
                case 'Eclético':
                    this.dt_ecletico++
                    break;
                case 'Ágil':
                    this.dt_agil++
                    break;
                case 'Bruto':
                    this.dt_bruto++
                    break;
                case 'Estrategista':
                    this.dt_estrategista++
                    break;
                case 'Range':
                    this.dt_range++
                    break;
                case 'Assassino':
                    this.dt_assassino++
                    break;
                case 'Prankster':
                    this.dt_prankster++
                    break;
            }
            switch (this.p2.damage_type){
                case 'Eclético':
                    this.dt_ecletico++
                    break;
                case 'Ágil':
                    this.dt_agil++
                    break;
                case 'Bruto':
                    this.dt_bruto++
                    break;
                case 'Estrategista':
                    this.dt_estrategista++
                    break;
                case 'Range':
                    this.dt_range++
                    break;
                case 'Assassino':
                    this.dt_assassino++
                    break;
                case 'Prankster':
                    this.dt_prankster++
                    break;
            }
            switch (this.p3.damage_type){
                case 'Eclético':
                    this.dt_ecletico++
                    break;
                case 'Ágil':
                    this.dt_agil++
                    break;
                case 'Bruto':
                    this.dt_bruto++
                    break;
                case 'Estrategista':
                    this.dt_estrategista++
                    break;
                case 'Range':
                    this.dt_range++
                    break;
                case 'Assassino':
                    this.dt_assassino++
                    break;
                case 'Prankster':
                    this.dt_prankster++
                    break;
            }
            if (this.dt_prankster == 3){
                this.reg+=2
                this.p1.atk+=2
                this.p2.atk+=2
                this.p3.atk+=2
                this.p1.spd+=2
                this.p2.spd+=2
                this.p3.spd+=2
                this.p1.def+=2
                this.p2.def+=2
                this.p3.def+=2
                this.vida_max+=2
            }
        },
        async oponentes(){
            const req = await fetch('http://localhost:3000/mons');
            const data = await req.json();

            this.vida_atual = this.vida_max = 10;
            this.op01.nome = ''
            this.op01.img = ''
            this.op02.nome = ''
            this.op02.img = ''
            this.op03.nome = ''
            this.op03.img = ''
            this.op_vida_max = 10
            this.op_vida_atual = 10
            this.op_reg = 0
            this.op_atk_max = 0
            this.op_def_max = 0
            this.op_spd_max = 0

            let lista = this.idmin

            let a = this.fase % 5
            if (a <= 1){
                let b = (Math.random()*lista).toFixed(0)
                let c = (Math.random()*lista).toFixed(0)
                let d = (Math.random()*lista).toFixed(0)

                this.op01.nome = data[0].grama[b].nome
                this.op01.img = data[0].grama[b].img
                this.op02.nome = data[1].fogo[c].nome
                this.op02.img = data[1].fogo[c].img
                this.op03.nome = data[2].agua[d].nome
                this.op03.img = data[2].agua[d].img
                this.op_atk_max += data[0].grama[b].atk
                this.op_def_max += data[0].grama[b].def
                this.op_spd_max += data[0].grama[b].spd
                this.op_atk_max += data[1].fogo[c].atk
                this.op_def_max += data[1].fogo[c].def
                this.op_spd_max += data[1].fogo[c].spd
                this.op_atk_max += data[2].agua[d].atk
                this.op_def_max += data[2].agua[d].def
                this.op_spd_max += data[2].agua[d].spd

            } else
            if (a <= 2) {
                let b = (Math.random()*lista).toFixed(0)
                let c = (Math.random()*lista).toFixed(0)
                let d = (Math.random()*lista).toFixed(0)

                this.op01.nome = data[0].grama[b].nome
                this.op01.img = data[0].grama[b].img
                this.op02.nome = data[1].fogo[c].nome
                this.op02.img = data[1].fogo[c].img
                this.op03.nome = data[2].agua[d].nome
                this.op03.img = data[2].agua[d].img
                this.op_atk_max += data[0].grama[b].atk
                this.op_def_max += data[0].grama[b].def
                this.op_spd_max += data[0].grama[b].spd
                this.op_atk_max += data[1].fogo[c].atk
                this.op_def_max += data[1].fogo[c].def
                this.op_spd_max += data[1].fogo[c].spd
                this.op_atk_max += data[2].agua[d].atk
                this.op_def_max += data[2].agua[d].def
                this.op_spd_max += data[2].agua[d].spd
                this.op_atk_max += 2
                this.op_def_max += 2
                this.op_spd_max += 2
                this.op_vida_max += 2
                this.op_vida_atual += 2
                this.op_reg = 2
            } else
            if (a <= 4) {
                let b = (Math.random()*lista).toFixed(0)
                let c = (Math.random()*lista).toFixed(0)
                let d = (Math.random()*lista).toFixed(0)

                this.op01.nome = data[0].grama[b].nome
                this.op01.img = data[0].grama[b].img
                this.op02.nome = data[1].fogo[c].nome
                this.op02.img = data[1].fogo[c].img
                this.op03.nome = data[2].agua[d].nome
                this.op03.img = data[2].agua[d].img
                this.op_atk_max += data[0].grama[b].atk
                this.op_def_max += data[0].grama[b].def
                this.op_spd_max += data[0].grama[b].spd
                this.op_atk_max += data[1].fogo[c].atk
                this.op_def_max += data[1].fogo[c].def
                this.op_spd_max += data[1].fogo[c].spd
                this.op_atk_max += data[2].agua[d].atk
                this.op_def_max += data[2].agua[d].def
                this.op_spd_max += data[2].agua[d].spd
                this.op_atk_max += 3
                this.op_def_max += 3
                this.op_spd_max += 2
                this.op_vida_max += 5
                this.op_vida_atual += 5
                this.op_reg = 2
            } else
            if (a <= 6) {
                let b = (Math.random()*lista).toFixed(0)
                let c = (Math.random()*lista).toFixed(0)
                let d = (Math.random()*lista).toFixed(0)

                this.op01.nome = data[0].grama[b].nome
                this.op01.img = data[0].grama[b].img
                this.op02.nome = data[1].fogo[c].nome
                this.op02.img = data[1].fogo[c].img
                this.op03.nome = data[2].agua[d].nome
                this.op03.img = data[2].agua[d].img
                this.op_atk_max += data[0].grama[b].atk
                this.op_def_max += data[0].grama[b].def
                this.op_spd_max += data[0].grama[b].spd
                this.op_atk_max += data[1].fogo[c].atk
                this.op_def_max += data[1].fogo[c].def
                this.op_spd_max += data[1].fogo[c].spd
                this.op_atk_max += data[2].agua[d].atk
                this.op_def_max += data[2].agua[d].def
                this.op_spd_max += data[2].agua[d].spd
                this.op_atk_max += 4
                this.op_def_max += 4
                this.op_spd_max += 3
                this.op_vida_max += 8
                this.op_vida_atual += 8
                this.op_reg = 4
            } else
            if (a <= 8) {
                let b = (Math.random()*lista).toFixed(0)
                let c = (Math.random()*lista).toFixed(0)
                let d = (Math.random()*lista).toFixed(0)

                this.op01.nome = data[0].grama[b].nome
                this.op01.img = data[0].grama[b].img
                this.op02.nome = data[1].fogo[c].nome
                this.op02.img = data[1].fogo[c].img
                this.op03.nome = data[2].agua[d].nome
                this.op03.img = data[2].agua[d].img
                this.op_atk_max += data[0].grama[b].atk
                this.op_def_max += data[0].grama[b].def
                this.op_spd_max += data[0].grama[b].spd
                this.op_atk_max += data[1].fogo[c].atk
                this.op_def_max += data[1].fogo[c].def
                this.op_spd_max += data[1].fogo[c].spd
                this.op_atk_max += data[2].agua[d].atk
                this.op_def_max += data[2].agua[d].def
                this.op_spd_max += data[2].agua[d].spd
                this.op_atk_max += 6
                this.op_def_max += 6
                this.op_spd_max += 5
                this.op_vida_max += 10
                this.op_vida_atual += 10
                this.op_reg = 5
            }
            if (this.tipo_veneno > 0){
                this.op_reg -= 0.5
            }

            setTimeout(()=>{
                this.combat()
            },700)
        },
        combat(){
            let dano = 0; let danoop, danoseu = 0
            if(this.dt_bruto == 3){
                this.spd_max = 9
            }
            if (this.dt_agil == 3){
                this.agil3();
            }
            if (this.checkVida()){
                if ((this.spd_max >= this.op_spd_max) || (this.dt_prankster == 3)){
                    /*Dano no inimigo*/
                    if(this.dt_bruto == 3){dano = this.atk_max - (this.op_def_max - 3); this.bruto3()}
                    else {dano = this.atk_max - this.op_def_max}
                    if(dano <= 0){dano = 1}
                    if(this.dt_estrategista == 3){dano += 0.5; this.estrategista3()}
                    if (this.dt_assassino > 0){
                        danoop += 5;
                        dano++
                        this.regenerar();
                    }
                    this.op_vida_atual -= dano
                    danoop += dano
                    
                    setTimeout(() => {
                        if (this.checkVida()){
                            /*Dano em voce*/
                            dano = this.op_atk_max - this.def_max
                            if(dano <= 0){dano = 1}
                            if(this.dt_estrategista == 3){dano -= 0.5; this.estrategista3()}
                            if (this.dt_ecletico == 3){
                                let r = (Math.random()*3).toFixed(0)
                                if (r != 2){
                                    this.vida_atual -= dano
                                } else {this.ecletico3()}
                            } else {
                                this.vida_atual -= dano
                            }
                            if (this.dt_range == 3){this.op_vida_atual -= (dano / 2); this.range3()}
                            danoseu += dano
                        }
                    }, 500);
                } else {
                    /*Dano em voce*/
                    dano = this.op_atk_max - this.def_max
                    if(dano <= 0){dano = 1}
                    if(this.dt_estrategista == 3){dano -= 0.5; this.estrategista3()}
                    if (this.dt_ecletico == 3){
                        let r = (Math.random()*3).toFixed(0)
                        if (r != 2){
                            this.vida_atual -= dano
                        } else {this.ecletico3()}
                    } else {
                        this.vida_atual -= dano
                    }
                    if (this.dt_range == 3){this.op_vida_atual -= (dano / 2); this.range3()}
                    danoseu += dano
                    
                    setTimeout(() => {
                        if (this.checkVida()){
                            /*Dano no inimigo*/
                            if(this.dt_bruto == 3){dano = this.atk_max - (this.op_def_max - 3); this.bruto3()}
                            else {dano = this.atk_max - this.op_def_max}
                            if(dano <= 0){dano = 1}
                            if(this.dt_estrategista == 3){dano += 0.5; this.estrategista3()}
                            if (this.dt_assassino > 0){
                                danoop += 5;
                                dano++
                                this.regenerar();
                            }
                            this.op_vida_atual -= dano
                            danoop += dano
                        }
                    }, 500);
                }
            }
            setTimeout(()=>{
                if (this.checkVida()){ 
                    if (danoseu > 1){this.regenerar()}
                    if (danoop > 1){this.op_regenerar()}
                    this.combat()
                } else if (this.op_vida_atual <= 0){
                    this.vitoria()
                } else {this.derrota()}
            },1000)
            
        },
        vitoria(){
            this.vida_atual = this.vida_max;
            this.op01.nome = ''
            this.op01.img = ''
            this.op02.nome = ''
            this.op02.img = ''
            this.op03.nome = ''
            this.op03.img = ''
            this.op_reg = 0
            this.op_atk_max = 0
            this.op_def_max = 0
            this.op_spd_max = 0
            if (this.fase == 50){
                this.$emit('vitoria')
            } else {
                this.$emit('fase1')
            }
            setTimeout(()=>{this.oponentes()}, 2000)
        },
        derrota(){
            window.alert('Você PERDEU!')
            this.$emit('lose')
        },
        checkVida(){
            return (this.vida_atual > 0 && this.op_vida_atual > 0)
        },
        regenerar(){
            if (this.dt_assassino > 0){
                this.vida_atual += 1
            }
            this.vida_atual += this.reg;
        }, op_regenerar(){
            this.op_vida_atual += this.op_reg;
        }, agil3(){
            document.getElementById('Agil').style.backgroundColor = '#10bbe6';
            document.getElementById('Agil').style.color = '#fff';
            let atk = this.atk_max - 1 - Number((this.spd_max / 7).toFixed(0));
            let dano = atk - this.op_def_max
            if(dano <= 0){dano = 1}
            this.op_vida_atual -= dano

            setTimeout(() => {
                    document.getElementById('Agil').style.backgroundColor = '#fff';
                    document.getElementById('Agil').style.color = '#000';
            },  400);
        }, estrategista3(){
            document.getElementById('Estrategista').style.backgroundColor = '#6816cc';
            document.getElementById('Estrategista').style.color = '#fff';
            setTimeout(() => {
                document.getElementById('Estrategista').style.backgroundColor = '#fff';
                document.getElementById('Estrategista').style.color = '#000';
            },  500);
        }, bruto3(){
            document.getElementById('Bruto').style.backgroundColor = '#5e508c';
            document.getElementById('Bruto').style.color = '#fff';
            setTimeout(() => {
                document.getElementById('Bruto').style.backgroundColor = '#fff';
                document.getElementById('Bruto').style.color = '#000';
            },  500);
        }, range3(){
            document.getElementById('Range').style.backgroundColor = '#7dd932';
            document.getElementById('Range').style.color = '#fff';
            setTimeout(() => {
                document.getElementById('Range').style.backgroundColor = '#fff';
                document.getElementById('Range').style.color = '#000';
            },  500);
        }, ecletico3(){
            document.getElementById('Ecletico').style.backgroundColor = '#ebeb1c';
            document.getElementById('Ecletico').style.color = '#fff';
            setTimeout(() => {
                document.getElementById('Ecletico').style.backgroundColor = '#fff';
                document.getElementById('Ecletico').style.color = '#000';
            },  500);
        }, assassino1(){
            document.getElementById('Assassino').style.backgroundColor = '#940011';
            document.getElementById('Assassino').style.color = '#fff';
            setTimeout(() => {
                document.getElementById('Assassino').style.backgroundColor = '#fff';
                document.getElementById('Assassino').style.color = '#000';
            },  500);
        },
        checkmin(){
            if(this.vitorias >= 1){
                this.idmin = 8
            }
            if (this.vitorias >= 3){
                this.idmin = 9
            }
        }
    },
    mounted() {
        setTimeout(()=>{this.getChars()}, 500)
        setTimeout(()=>{this.oponentes()}, 900)
        this.checkmin();
    },
}
</script>

<style scoped>
.progress {background: rgba(0, 0, 0, 0);}
.progress-bar{background: green;}
.roxo {background: #6610f2;}
</style>