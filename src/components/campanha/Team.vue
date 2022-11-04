<template>
    <div style="overflow:hidden; user-select: none;">
        <div class="d-flex justify-content-center">
            <strong class="m-3 mt-1 mb-1">Vida: <span>({{vida_max}})</span></strong>
            <div class="row w-75 container">
                <div class="progress" :class="[{'col-11': reg != 0},{'col-12': reg == 0}]">
                    <div class="progress-bar" role="progressbar" :style="{ width: (vida_atual/vida_max)*100 > 0 ? (vida_atual/vida_max)*100 + '%' : 0 }" style="border-radius:10px;"></div>
                </div>
                <div class="col-1"><span v-if="reg != 0">+{{reg}}</span></div>
            </div>
        </div>
        <div class="d-flex justify-content-center col flex-wrap" style="overflow:hidden; user-select: none;"> 
            <table class="table w-75 table-bordered">
                <tr>
                    <th>Tipo</th>
                    <td :style="[{backgroundColor: (tipo_grama == 3 || (this.tipo_grama == 1 && this.tipo_fogo == 1 && this.tipo_agua == 1) || (this.tipo_grama == 2 && this.tipo_veneno == 1)) ? '#00e613' : '#fff' },{width: ((tipo_eletrico > 0) || (tipo_normal > 0) || (tipo_sombrio > 0) || (tipo_veneno > 0)) ? '20%' : '25%'}]">Grama: {{ tipo_grama }}</td>
                    <td :style="[{backgroundColor: (tipo_fogo == 3 || (this.tipo_grama == 1 && this.tipo_fogo == 1 && this.tipo_agua == 1) || (this.tipo_fogo == 2 && this.tipo_sombrio == 1)) ? '#e62600' : '#fff'},{width: ((tipo_eletrico > 0) || (tipo_normal > 0) || (tipo_sombrio > 0) || (tipo_veneno > 0)) ? '20%' : '25%'}]">Fogo: {{ tipo_fogo }}</td>
                    <td :style="[{backgroundColor: (tipo_agua == 3 || (this.tipo_grama == 1 && this.tipo_fogo == 1 && this.tipo_agua == 1) || (this.tipo_agua == 2 && this.tipo_eletrico == 1)) ? '#0425de' : '#fff'},{width: ((tipo_eletrico > 0) || (tipo_normal > 0) || (tipo_sombrio > 0) || (tipo_veneno > 0)) ? '20%' : '25%'}]">Água: {{ tipo_agua }}</td>
                    <td v-if="tipo_eletrico > 0" style="background-color: #d3eb00; width:20%;">Elétrico</td>
                    <td v-if="tipo_normal > 0" style="background-color: #9c9994; width:20%;">Normal</td>
                    <td v-if="tipo_sombrio > 0" style="background-color: #0c0c17; color: #ddd; width:20%;">Sombrio</td>
                    <td v-if="tipo_veneno > 0" style="background-color: #8409e8; width:20%;">Veneno</td>
                </tr>
            </table>
            <table class="table w-75 table-bordered" style="margin-top: -17px;">
                <tr>
                    <th>Classe</th>
                    <td :style="[{backgroundColor: (c_atk == 3 || (this.c_atk == 1 && this.c_tank == 1 && this.c_sup == 1)) ? '#f52020' : '#fff' },{width: c_pro > 0 ? '20%' : '25%'}]">Atacante: {{ c_atk }}</td>
                    <td :style="[{backgroundColor: (c_tank == 3 || (this.c_atk == 1 && this.c_tank == 1 && this.c_sup == 1)) ? '#0f0382' : '#fff'},{width: c_pro > 0 ? '20%' : '25%'},{color: dt_agil == 3 ? '#fff' : '#000'}]">Tank: {{ c_tank }}</td>
                    <td :style="[{backgroundColor: (c_sup == 3 || (this.c_atk == 1 && this.c_tank == 1 && this.c_sup == 1)) ? '#f21189' : '#fff' },{width: c_pro > 0 ? '20%' : '25%'}]">Suporte: {{ c_sup }}</td>
                    <td v-if="c_pro > 0" style="width:20%;">Protagonista <span v-if="c_pro >= 2">- II</span></td> 
                </tr>
            </table>
            <table class="table w-75 table-bordered" style="margin-top: -17px;">
                <tr>
                    <th>Tipo de Dano</th>
                    <td :style="[{backgroundColor: dt_ecletico == 3 ? '#ebeb1c' : '#fff' },{width: (dt_assassino > 0 || dt_prankster == 3) ? '14%' : '16%'}]">Eclético: {{ dt_ecletico }}</td>
                    <td :style="[{backgroundColor: dt_agil == 3 ? '#10bbe6' : '#fff'},{color: dt_agil == 3 ? '#fff' : '#000'},{width: (dt_assassino > 0 || dt_prankster == 3) ? '14%' : '16%'}]">Ágil: {{ dt_agil }}</td>
                    <td :style="[{backgroundColor: dt_estrategista == 3 ? '#6816cc' : '#fff'},{color: dt_estrategista == 3 ? '#fff' : '#000'},{width: (dt_assassino > 0 || dt_prankster == 3) ? '14%' : '16%'}]">Estrategista: {{ dt_estrategista }}</td>
                    <td :style="[{backgroundColor: dt_bruto == 3 ? '#5e508c' : '#fff'},{color: dt_bruto == 3 ? '#fff' : '#000'},{width: (dt_assassino > 0 || dt_prankster == 3) ? '14%' : '16%'}]">Bruto: {{ dt_bruto }}</td>
                    <td :style="[{backgroundColor: dt_range == 3 ? '#7dd932' : '#fff'},{color: dt_range == 3 ? '#fff' : '#000'},{width: (dt_assassino > 0 || dt_prankster == 3) ? '14%' : '16%'}]">Range: {{ dt_range }}</td>
                    <td v-if="dt_assassino > 0" style="background-color: #940011; color:#fff; width:14%;">Assassino: {{ dt_assassino }}</td>
                    <td v-if="dt_prankster == 3" style="background-color: #ff6229; color:#2200ba; width:14%;">Prankster: {{ dt_prankster }}</td>
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
                        <img :src="'/icons/' + p1.img" class="card-img-top" v-if="p1.img != 'placeholder'" alt="..." style="width: 200px; left:auto; right: auto; margin:auto">
                        <h5 class="card-title">{{ p1.nome }}</h5>
                        <h6 class="card-subtitle text-muted" v-if="p1.img != 'placeholder'"><strong>[{{ p1.tipo }}]</strong> - {{ p1.classe }} | {{ p1.damage_type }}</h6>
                        <ul class="list-group">
                            <li class="list-group-item">Ataque: {{ p1.atk }}</li>
                            <li class="list-group-item">Defesa: {{ p1.def }}</li>
                            <li class="list-group-item">Velocidade: {{ p1.spd }}</li>
                        </ul>
                    </div>
                    <div class="card-body" :class="{ imgempty: p2.img == 'placeholder' } ">
                        <img :src="'/icons/' + p2.img" class="card-img-top" v-if="p2.img != 'placeholder'" alt="..." style="width: 200px; left:auto; right: auto; margin:auto">
                        <h5 class="card-title">{{ p2.nome }}</h5>
                        <h6 class="card-subtitle text-muted" v-if="p2.img != 'placeholder'"><strong>[{{ p2.tipo }}]</strong> - {{ p2.classe }} | {{ p2.damage_type }}</h6>
                        <ul class="list-group">
                            <li class="list-group-item">Ataque: {{ p2.atk }}</li>
                            <li class="list-group-item">Defesa: {{ p2.def }}</li>
                            <li class="list-group-item">Velocidade: {{ p2.spd }}</li>
                        </ul>
                    </div>
                    <div class="card-body" :class="{ imgempty: p3.img == 'placeholder' } ">
                        <img :src="'/icons/' + p3.img" class="card-img-top" v-if="p3.img != 'placeholder'" alt="..." style="width: 200px; left:auto; right: auto; margin:auto">
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

            this.p1 = data[0].p
            this.p2 = data[1].p
            this.p3 = data[2].p

            this.checarBonus();
        },
        checarBonus(){
            this.checkTipo();
            this.checkClass();
            this.checkDT();
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

    },
    mounted() {
        this.getChars();
        setTimeout(() => {
            this.$emit("page1")
        }, 2000);
    },
}
</script>

<style scoped>
.imgempty{
    padding-top:263px;
}

.progress {background: rgba(0, 0, 0, 0);}
.progress-bar{background: green;}
</style>