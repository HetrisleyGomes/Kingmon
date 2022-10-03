<template>
    <div style="overflow-x:hidden; margin-top: -20px;">
        {{prop}}
        <div class="d-flex justify-content-center row p-3">
            <div class="col" style="width: 90%; left:auto; right: auto; margin:auto">
                <strong class="m-3 mt-1 mb-1">Vida do Time:</strong>
                <progress :value="vida_atual" :max="vida_max" class="progress-bar w-100" style="border-radius: 10px;"></progress>
                <span v-if="reg != 0">+{{reg}}</span>
            </div>
            <div style="display:flex; width: 50px; text-align: left; left:auto; right: auto; margin:auto; margin-bottom:-10px;"><h4>{{fase}}</h4></div>
            <div class="col" style="width: 90%; left:auto; right: auto; margin:auto">
                <strong class="m-3 mt-1 mb-1">Vida do Oponente:</strong>
                <progress :value="op_vida_atual" :max="op_vida_max" class="progress-bar w-100" style="border-radius: 10px;"></progress>
                <span v-if="op_reg != 0">+{{op_reg}}</span>
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
                            <img :src="p1.img" class="card-img-top" v-if="p1.img != 'placeholder'" alt="..." style="width: 200px; left:auto; right: auto; margin:auto">
                            <h5 class="card-title">{{ p1.nome }}</h5>
                        </div>
                        <div class="card-body" v-if="p2.img">
                            <img :src="p2.img" class="card-img-top" v-if="p2.img != 'placeholder'" alt="..." style="width: 200px; left:auto; right: auto; margin:auto">
                            <h5 class="card-title">{{ p2.nome }}</h5>
                        </div>
                        <div class="card-body" v-if="p3.img">
                            <img :src="p3.img" class="card-img-top" v-if="p3.img != 'placeholder'" alt="..." style="width: 200px; left:auto; right: auto; margin:auto">
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
                            <img :src="op01.img" class="card-img-top" v-if="op01.img != 'placeholder'" alt="..." style="width: 200px; left:auto; right: auto; margin:auto; transform: scaleX(-1); filter: hue-rotate(45deg)">
                            <h5 class="card-title">{{ op01.nome }}</h5>
                        </div>
                        <div class="card-body" v-if="op02.img">
                            <img :src="op02.img" class="card-img-top" v-if="op02.img != 'placeholder'" alt="..." style="width: 200px; left:auto; right: auto; margin:auto; transform: scaleX(-1); filter: hue-rotate(45deg)">
                            <h5 class="card-title">{{ op02.nome }}</h5>
                        </div>
                        <div class="card-body" v-if="op03.img">
                            <img :src="op03.img" class="card-img-top" v-if="op03.img != 'placeholder'" alt="..." style="width: 200px; left:auto; right: auto; margin:auto; transform: scaleX(-1); filter: hue-rotate(45deg)">
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
            tipo_extra: 0,
            dt_ecletico: 0,
            dt_agil: 0,
            dt_estrategista: 0,
            dt_bruto: 0,
            dt_range: 0,
            dt_assassino: 0,
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
        }
    }, props:{
        fase: Number,
        vitorias: Number,
    },
    methods: {
        async getChars(){
            const req = await fetch('http://localhost:3000/time');
            const data = await req.json();
            this.p1 = data[0].p1
            this.p2 = data[1].p2
            this.p3 = data[2].p3
            this.checarBonus();
        },
        checarBonus(){ 
            this.checkTipo();
            this.checkClass();
            this.checkDT();
            this.calcmax();
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
                case 'Extra':
                    this.tipo_extra++
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
                case 'Extra':
                    this.tipo_extra++
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
                case 'Extra':
                    this.tipo_extra++
                    break;
            }

            if ((this.tipo_grama == 3) || ((this.tipo_grama == 2) && (this.tipo_extra == 1))) {
                this.p1.atk++
                this.p2.atk++
                this.p3.atk++
                this.reg++
            }
            if ((this.tipo_fogo == 3) || ((this.tipo_fogo == 2) && (this.tipo_extra == 1))) {
                this.p1.atk++
                this.p2.atk++
                this.p3.atk++
                this.p1.spd++
                this.p2.spd++
                this.p3.spd++
            }
            if ((this.tipo_agua == 3) || ((this.tipo_agua == 2) && (this.tipo_extra == 1))) {
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
            }
        },
        checkClass(){
            let p = 0;
            switch (this.p1.classe){
                case 'Atacante':
                    this.c_atk++
                    break;
                case 'Protagonista':
                    this.c_atk++
                    this.c_sup++
                    this.c_pro++
                    p = 1
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
                    this.c_atk++
                    this.c_sup++
                    this.c_pro++
                    p = 2
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
                    this.c_atk++
                    this.c_sup++
                    this.c_pro++
                    p = 3
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
                this.vida_max+=2
            }
            if (this.c_atk == 1 && this.c_tank == 1 && this.c_sup == 1 && this.c_pro == 0){
                this.p1.def++
                this.p2.atk++
                this.p3.spd++
                this.reg++
            } 
            if (this.c_atk > 0 && this.c_tank > 0 && this.c_sup > 1 && this.c_pro > 0){
                this.p1.atk++
                this.p2.atk++
                this.p3.atk++
                this.p1.def++
                this.p2.def++
                this.p3.def++
                this.reg++
                if (p == 1){
                    this.p1.def++
                } else if (p == 2){
                    this.p2.def++
                } else if (p == 3){
                    this.p3.def++

                }
            } else if (this.c_pro > 0){
                if (p == 1){
                    this.p1.def++
                    this.p1.spd++
                } else if (p == 2){
                    this.p2.def+=2
                } else if (p == 3){
                    this.p3.def++
                    this.p2.atk++
                }
                this.reg+=0.5
            }
        },
        checkDT(){
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

            let lista
            if (this.vitorias == 0){lista = 7} 
            else {lista = 8}

            let a = this.fase
            if (a <= 2){
                let b = (Math.random()*2).toFixed(0)
                let c = (Math.random()*lista).toFixed(0)
                if (b == 0){
                    this.op01.nome = data[0].grama[c].nome
                    this.op01.img = data[0].grama[c].img
                    this.op_atk_max += data[0].grama[c].atk
                    this.op_def_max += data[0].grama[c].def
                    this.op_spd_max += data[0].grama[c].spd
                }
                if (b == 1){
                    this.op02.nome = data[1].fogo[c].nome
                    this.op02.img = data[1].fogo[c].img
                    this.op_atk_max += data[1].fogo[c].atk
                    this.op_def_max += data[1].fogo[c].def
                    this.op_spd_max += data[1].fogo[c].spd
                }
                if (b == 2){
                    this.op03.nome = data[2].agua[c].nome
                    this.op03.img = data[2].agua[c].img
                    this.op_atk_max += data[2].agua[c].atk
                    this.op_def_max += data[2].agua[c].def
                    this.op_spd_max += data[2].agua[c].spd
                }
            } else
            if (a <= 4){
                let b = (Math.random()*2).toFixed(0)
                let c = (Math.random()*lista).toFixed(0)
                let d = (Math.random()*lista).toFixed(0)
                if (b == 0){
                    this.op01.nome = data[0].grama[c].nome
                    this.op01.img = data[0].grama[c].img
                    this.op02.nome = data[1].fogo[d].nome
                    this.op02.img = data[1].fogo[d].img
                    this.op_atk_max += data[0].grama[c].atk
                    this.op_def_max += data[0].grama[c].def
                    this.op_spd_max += data[0].grama[c].spd
                    this.op_atk_max += data[1].fogo[d].atk
                    this.op_def_max += data[1].fogo[d].def
                    this.op_spd_max += data[1].fogo[d].spd
                }
                if (b == 1){
                    this.op02.nome = data[1].fogo[c].nome
                    this.op02.img = data[1].fogo[c].img
                    this.op03.nome = data[2].agua[d].nome
                    this.op03.img = data[2].agua[d].img
                    this.op_atk_max += data[1].fogo[c].atk
                    this.op_def_max += data[1].fogo[c].def
                    this.op_spd_max += data[1].fogo[c].spd
                    this.op_atk_max += data[2].agua[d].atk
                    this.op_def_max += data[2].agua[d].def
                    this.op_spd_max += data[2].agua[d].spd
                }
                if (b == 2){
                    this.op03.nome = data[2].agua[c].nome
                    this.op03.img = data[2].agua[c].img
                    this.op01.nome = data[0].grama[d].nome
                    this.op01.img = data[0].grama[d].img
                    this.op_atk_max += data[2].agua[c].atk
                    this.op_def_max += data[2].agua[c].def
                    this.op_spd_max += data[2].agua[c].spd
                    this.op_atk_max += data[0].grama[d].atk
                    this.op_def_max += data[0].grama[d].def
                    this.op_spd_max += data[0].grama[d].spd
                }
            } else
            if (a <= 10){
                let b = (Math.random()*lista).toFixed(0)
                let c = (Math.random()*lista).toFixed(0)
                let d = (Math.random()*lista).toFixed(0)
                let r = (Math.random()*4).toFixed(0)
                if (r >= 3){
                    this.op_reg = 1
                }

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
            if (a <= 14) {
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
                this.op_vida_max += 3
                this.op_vida_atual += 3
                this.op_reg = 1
            } else {
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
                this.op_def_max += 4
                this.op_spd_max += 3
                this.op_vida_max += 5
                this.op_vida_atual += 5
                this.op_reg = 2
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
                if (this.spd_max >= this.op_spd_max){
                    /*Dano no inimigo*/
                    if(this.dt_bruto == 3){dano = this.atk_max - (this.op_def_max - 2); this.bruto3()}
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
                            if(this.dt_bruto == 3){dano = this.atk_max - (this.op_def_max - 2); this.bruto3()}
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
            if (this.fase == 15){
                this.$emit('vitoria')
            } else {
                this.$emit('fase1')
                this.$emit('page2')
            }
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
        }
    },
    mounted() {
        setTimeout(()=>{this.getChars()}, 500)
        setTimeout(()=>{this.oponentes()}, 900)
    },
}
</script>