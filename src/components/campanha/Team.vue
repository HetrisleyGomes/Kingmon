<template>
    <div style="overflow:hidden; user-select: none;">
        <div class="d-flex justify-content-center">
            <strong class="m-3 mt-1 mb-1">Vida:</strong>
            <progress :value="vida_atual" :max="vida_max" class="progress-bar w-50 m-3 mt-1 mb-1" style="border-radius: 10px;"></progress>
            <span v-if="reg != 0">+{{reg}}</span>
            <input type="button" class="btn btn-secondary btn-sm" value="Continuar" @click="$emit('page1')" style="margin-bottom:5px; margin-left: 5px;">
        </div>
        <div class="d-flex justify-content-center col flex-wrap" style="overflow:hidden; user-select: none;"> 
            <table class="table w-75 table-bordered">
                <tr>
                    <th style="width:25%">Tipo</th>
                    <td v-if="tipo_grama == 3 || (this.tipo_grama == 1 && this.tipo_fogo == 1 && this.tipo_agua == 1)" style="background-color: #00e613; width:25%">Grama: {{ tipo_grama }}</td>
                    <td v-else style="width:25%">Grama: {{ tipo_grama }}</td>
                    <td v-if="tipo_fogo == 3 || (this.tipo_grama == 1 && this.tipo_fogo == 1 && this.tipo_agua == 1)" style="background-color: #e62600; width:25%">Fogo: {{ tipo_fogo }}</td>
                    <td v-else style="width:25%">Fogo: {{ tipo_fogo }}</td>
                    <td v-if="tipo_agua == 3 || (this.tipo_grama == 1 && this.tipo_fogo == 1 && this.tipo_agua == 1)" style="background-color: #0425de; width:25%">Água: {{ tipo_agua }}</td>
                    <td v-else style="width:25%">Água: {{ tipo_agua }}</td>
                    <td v-if="tipo_extra > 0">Extra</td>
                </tr>
                <tr>
                    <th>Classe</th>
                    
                    <td v-if="c_atk == 3 || (this.c_atk == 1 && this.c_tank == 1 && this.c_sup == 1)" style="background-color: #f52020;">Atacante: {{ c_atk }}</td>
                    <td v-else>Atacante: {{ c_atk }}</td>
                    <td v-if="c_tank == 3 || (this.c_atk == 1 && this.c_tank == 1 && this.c_sup == 1)" style="background-color: #0f0382; color: #fff">Tank: {{ c_tank }}</td>
                    <td v-else>Tank: {{ c_tank }}</td>
                    <td v-if="c_sup == 3 || (this.c_atk == 1 && this.c_tank == 1 && this.c_sup == 1)" style="background-color: #f21189;">Suporte: {{ c_sup }}</td>
                    <td v-else>Suporte: {{ c_sup }}</td>
                    <td v-if="c_pro > 0">Protagonista</td>
                    
                </tr>
            </table>
            <table class="table w-75 table-bordered" style="margin-top: -17px;">
                <tr>
                    <th>Tipo de Dano</th>
                    <td v-if="dt_ecletico != 3">Eclético: {{ dt_ecletico }}</td>
                    <td v-else style="background-color: #ebeb1c;">Eclético: {{ dt_ecletico }}</td>
                    <td v-if="dt_agil != 3">Ágil: {{ dt_agil }}</td>
                    <td v-else style="background-color: #10bbe6; color:#fff">Ágil: {{ dt_agil }}</td>
                    <td v-if="dt_estrategista != 3">Estrategista: {{ dt_estrategista }}</td>
                    <td v-else style="background-color: #6816cc; color:#fff">Estrategista: {{ dt_estrategista }}</td>
                    <td v-if="dt_bruto != 3">Bruto: {{ dt_bruto }}</td>
                    <td v-else style="background-color: #5e508c; color:#fff">Bruto: {{ dt_bruto }}</td>
                    <td v-if="dt_range != 3">Range: {{ dt_range }}</td>
                    <td v-else style="background-color: #7dd932; color:#fff">Range: {{ dt_range }}</td>
                    <td v-if="dt_assassino > 0" style="background-color: #940011; color:#fff">Assassino: {{ dt_assassino }}</td>
                </tr>
            </table>
        </div>
        <div class="d-flex justify-content-center p-3" style="margin-top: -17px;">
            <table class="table w-75">
                <tr>

                    <td v-if="dt_agil == 3">Bônus Ágil: Inflige um dano um pouco mais frágil antes do combate começar</td>
                    <td v-else-if="dt_ecletico == 3">Bônus Eclético: Tem uma chance de esquivar do ataque do oponente</td>
                    <td v-else-if="dt_estrategista == 3">Bônus Estrategista: Ganha um pequeno bônus de ataque e defesa</td>
                    <td v-else-if="dt_range == 3">Bônus Range: Quando atacado, inflige metade do dano ao oponente</td>
                    <td v-else-if="dt_bruto == 3">Bônus Bruto: Ao diminuir a velocidade, aumenta o seu poder de ataque</td>
                    <td v-if="dt_assassino > 0">Bônus Assassino: Impede que o oponente se regenere, além de curar 1 PV a cada turno.</td>

                </tr>
            </table>
        </div>
        <div class="d-flex justify-content-center" style="margin-top: -17px;">
            <div class="card position-fixed " style="width: 80%; left:auto; right: auto; margin:auto">
                <input type="hidden" value="{{item}}">
                <div class="d-flex">
                    <div class="card-body" :class="{ imgempty: p1.img == 'placeholder'} ">
                        <img :src="p1.img" class="card-img-top" v-if="p1.img != 'placeholder'" alt="..." style="width: 200px; left:auto; right: auto; margin:auto">
                        <h5 class="card-title">{{ p1.nome }}</h5>
                        <h6 class="card-subtitle text-muted" v-if="p1.img != 'placeholder'"><strong>[{{ p1.tipo }}]</strong> - {{ p1.classe }} | {{ p1.damage_type }}</h6>
                        <ul class="list-group">
                            <li class="list-group-item">Ataque: {{ p1.atk }}</li>
                            <li class="list-group-item">Defesa: {{ p1.def }}</li>
                            <li class="list-group-item">Velocidade: {{ p1.spd }}</li>
                        </ul>
                    </div>
                    <div class="card-body" :class="{ imgempty: p2.img == 'placeholder' } ">
                        <img :src="p2.img" class="card-img-top" v-if="p2.img != 'placeholder'" alt="..." style="width: 200px; left:auto; right: auto; margin:auto">
                        <h5 class="card-title">{{ p2.nome }}</h5>
                        <h6 class="card-subtitle text-muted" v-if="p2.img != 'placeholder'"><strong>[{{ p2.tipo }}]</strong> - {{ p2.classe }} | {{ p2.damage_type }}</h6>
                        <ul class="list-group">
                            <li class="list-group-item">Ataque: {{ p2.atk }}</li>
                            <li class="list-group-item">Defesa: {{ p2.def }}</li>
                            <li class="list-group-item">Velocidade: {{ p2.spd }}</li>
                        </ul>
                    </div>
                    <div class="card-body" :class="{ imgempty: p3.img == 'placeholder' } ">
                        <img :src="p3.img" class="card-img-top" v-if="p3.img != 'placeholder'" alt="..." style="width: 200px; left:auto; right: auto; margin:auto">
                        <h5 class="card-title">{{ p3.nome }}</h5>
                        <h6 class="card-subtitle text-muted" v-if="p3.img != 'placeholder'"><strong>[{{ p3.tipo }}]</strong> - {{ p3.classe }} | {{ p3.damage_type }}</h6>
                        <ul class="list-group">
                            <li class="list-group-item">Ataque: {{ p3.atk }}</li>
                            <li class="list-group-item">Defesa: {{ p3.def }}</li>
                            <li class="list-group-item">Velocidade: {{ p3.spd }}</li>
                        </ul>
                    </div>
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
                this.vida_max++
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
        }

    },
    mounted() {
        this.getChars();
    },
}
</script>

<style scoped>
.imgempty{
    padding-top:263px;
}
</style>