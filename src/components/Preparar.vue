<template>
    <div class="container" style="user-select: none;">
        <div class="row">
            <div class="col">
                <div v-for="item in grama" :key="item.id" class="border mb-2">
                    <div class="row" v-if="item.id <= idmin"><span class="col"><strong>{{ item.nome }}</strong></span><span class="col"><img :src="'/icons/' + item.img"></span></div>
                    <div class="row" v-if="item.id <= idmin"><span class="col"><strong>{{ item.tipo }}</strong></span><span class="col">{{ item.classe }}</span><span class="col">{{ item.damage_type }}</span></div>
                    <div class="row"><span class="col">Atk: {{ item.atk }}</span><span class="col">Def: {{ item.def }}</span><span class="col">Spd: {{ item.spd }}</span></div>
                </div>
            </div>
            <div class="col">
                <div v-for="item in fogo" :key="item.id" class="border mb-2">
                    <div class="row" v-if="item.id <= idmin"><span class="col"><strong>{{ item.nome }}</strong></span><span class="col"><img :src="'/icons/' + item.img"></span></div>
                    <div class="row" v-if="item.id <= idmin"><span class="col"><strong>{{ item.tipo }}</strong></span><span class="col">{{ item.classe }}</span><span class="col">{{ item.damage_type }}</span></div>
                    <div class="row"><span class="col">Atk: {{ item.atk }}</span><span class="col">Def: {{ item.def }}</span><span class="col">Spd: {{ item.spd }}</span></div>
                </div>
            </div>
            <div class="col">
                <div v-for="item in agua" :key="item.id" class="border mb-2">
                    <div class="row" v-if="item.id <= idmin"><span class="col"><strong>{{ item.nome }}</strong></span><span class="col"><img :src="'/icons/' + item.img"></span></div>
                    <div class="row" v-if="item.id <= idmin"><span class="col"><strong>{{ item.tipo }}</strong></span><span class="col">{{ item.classe }}</span><span class="col">{{ item.damage_type }}</span></div>
                    <div class="row"><span class="col">Atk: {{ item.atk }}</span><span class="col">Def: {{ item.def }}</span><span class="col">Spd: {{ item.spd }}</span></div>
                </div>
            </div>
        </div>
        <div v-if="vitorias >= 6" class="row mt-2">
            <div class="col border" v-for="item in extras" :key="item.id" style="width:20%;">
                <div class="row"><span class="col p-0"><strong>{{ item.nome }}</strong></span> <span class="col"><img :src="'/icons/' + item.img"></span></div>
                <div class="row"><span class="col"><strong>{{ item.tipo }}</strong> </span><span class="col p-0"> {{ item.classe }} </span><span class="col"> {{ item.damage_type }}</span></div>
                <div class="row ms-3 me-3"><span class="col">Atk: {{ item.atk }} </span><span class="col"> Def: {{ item.def }} </span><span class="col"> Spd: {{ item.spd }}</span></div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Preparar',
    data(){
        return{
            mons: null,
            grama: null,
            fogo: null,
            agua: null,
            extras: null,
            idmin: 8,
            vitorias: 0,
            dez: 10,
            sete: 19,
        }
    },
    methods: {
        async getMons(){
            const req = await fetch('http://localhost:3000/mons');
            const data = await req.json();
            this.grama = data[0].grama
            this.fogo = data[1].fogo
            this.agua = data[2].agua
            this.extras = data[3].extra
        },
        async getBonus(){
            const req = await fetch('http://localhost:3000/bonus');
            const data = await req.json();
            this.vitorias = data[1].extra.vitorias
            setTimeout(this.checkmin(),500)
        },
        checkmin(){
            if(this.vitorias >= 1){
                this.idmin = 9
            }
            if (this.vitorias >= 3){
                this.idmin = 10
            }
        }
    },
    mounted() {
        this.getMons();
        this.getBonus();
    },
}
</script>

<style scoped>

span button {
    width: 80px;
    color: white;
    border: none;
    padding: 5px 10px;
    margin: 2px;
    border-radius: 40px;
    text-align: center;
}
.btn-grama {
    background-color: green; 
}
.btn-fogo {
    background-color: orangered; 
}
.btn-agua {
    background-color: #0046df; 
}
.btn-extras {
    background-color: #08090d; 
    color: #fff;
}
span img {
    width: 100px;
}
span {
    text-align: center;
    margin: auto;
}
.bckgrnd{
    background: #000;
    opacity: 0.9;
    z-index: -5;
    height: 100%;
    width: 100%;
    pointer-events: none;
    
}
.bckgrnd button {
    background-color: #1f1f1f; 
}
.frente{
    z-index: 900;
}
.imgempty{
    padding-top:263px;
}
.btn-caixa{
    width: 200px;
    margin: 20px;
}
</style>