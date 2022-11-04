<template>
    <div style="overflow-x:hidden; user-select: none;">
        <div v-if="!esconder" class="container position-relative d-flex justify-content-center frente">
            <div class="card position-fixed " style="width: 80%; left:auto; right: auto; margin:auto">
                <img :src="'/icons/' + mon.img" class="card-img-top" alt="..." style="width: 200px; left:auto; right: auto; margin:auto">
                <div class="card-body">
                    <h5 class="card-title">{{ mon.nome }}</h5>
                    <h6 class="card-subtitle mb-2 text-muted"><strong>[{{ mon.tipo }}]</strong> - {{ mon.classe }} | {{ mon.damage_type }}</h6>
                    <p class="card-text">{{ mon.desc }}</p>
                    <ul class="list-group m-3">
                        <li class="list-group-item">Ataque: {{ mon.atk }}</li>
                        <li class="list-group-item">Defesa: {{ mon.def }}</li>
                        <li class="list-group-item">Velocidade: {{ mon.spd }}</li>
                    </ul>
                    <button class="btn btn-danger w-25" style="width: 200px; left:auto; right: auto; margin:auto" @click="esconder = true">fechar</button>
                </div>
            </div>
        </div>
        <div v-if="!esconder_escolher" class="container position-relative d-flex justify-content-center frente">
            <div class="card position-fixed " style="width: 80%; left:auto; right: auto; margin:auto">
                <input type="hidden" value="{{item}}">
                <div class="d-flex">
                    <div class="card-body" :class="{ imgempty: p1.img=='placeholder' } ">
                        <img :src="'/icons/' + p1.img" class="card-img-top" v-if="p1.img != 'placeholder'" alt="..." style="width: 200px; left:auto; right: auto; margin:auto">
                        <h5 class="card-title">{{ p1.nome }}</h5>
                        <ul class="list-group m-3">
                            <li class="list-group-item">Ataque: {{ p1.atk }}</li>
                            <li class="list-group-item">Defesa: {{ p1.def }}</li>
                            <li class="list-group-item">Velocidade: {{ p1.spd }}</li>
                        </ul>
                        <button class="btn btn-primary" v-if="p1.img == 'placeholder'" @click="escolherp1(mon)">Escolher</button>
                        <button class="btn btn-primary" v-else @click="escolherp1(mon)">Substituir</button>
                    </div>
                    <div class="card-body" :class="{ imgempty: p2.img=='placeholder' } ">
                        <img :src="'/icons/' + p2.img" class="card-img-top" v-if="p2.img != 'placeholder'" alt="..." style="width: 200px; left:auto; right: auto; margin:auto">
                        <h5 class="card-title">{{ p2.nome }}</h5>
                        <ul class="list-group m-3">
                            <li class="list-group-item">Ataque: {{ p2.atk }}</li>
                            <li class="list-group-item">Defesa: {{ p2.def }}</li>
                            <li class="list-group-item">Velocidade: {{ p2.spd }}</li>
                        </ul>
                        <button class="btn btn-primary" v-if="p2.img == 'placeholder'" @click="escolherp2(mon)">Escolher</button>
                        <button class="btn btn-primary" v-else @click="escolherp2(mon)">Substituir</button>
                    </div>
                    <div class="card-body" :class="{ imgempty: p3.img=='placeholder' } ">
                        <img :src="'/icons/' + p3.img" class="card-img-top" v-if="p3.img != 'placeholder'" alt="..." style="width: 200px; left:auto; right: auto; margin:auto">
                        <h5 class="card-title">{{ p3.nome }}</h5>
                        <ul class="list-group m-3">
                            <li class="list-group-item">Ataque: {{ p3.atk }}</li>
                            <li class="list-group-item">Defesa: {{ p3.def }}</li>
                            <li class="list-group-item">Velocidade: {{ p3.spd }}</li>
                        </ul>
                        <button class="btn btn-primary" v-if="p3.img == 'placeholder'" @click="escolherp3(mon)">Escolher</button>
                        <button class="btn btn-primary" v-else @click="escolherp3(mon)">Substituir</button>
                    </div>
                </div>
                <button class="btn btn-danger w-25" @click="esconder_escolher = true" style="width: 200px; left:auto; right: auto; margin:auto">fechar</button>
            </div>
            
        </div>
        <div class="row" :class="[{ bckgrnd: !esconder },{ bckgrnd: !esconder_escolher }]">
            <div class="col">
                <div v-for="item in grama" :key="item.id">
                    <div class="row" v-if="item.id <= idmin"><span class="col"><strong>{{ item.nome }}</strong></span> <span class="col"><img :src="'/icons/' + item.img"></span> <span class="col"><button type="button" class="btn-grama" @click="escolher(item)">Escolher</button><br/><button type="button" class="btn-ver" @click="vermon(item)">Ver</button></span></div>
                </div>
            </div>
            <div class="col">
                <div v-for="item in fogo" :key="item.id">
                    <div class="row" v-if="item.id <= idmin"><span class="col"><strong>{{ item.nome }}</strong></span> <span class="col"><img :src="'/icons/' + item.img"></span> <span class="col"><button type="button" class="btn-fogo" @click="escolher(item)">Escolher</button><br/><button type="button" class="btn-ver" @click="vermon(item)">Ver</button></span></div>
                </div>
            </div>
            <div class="col">
                <div v-for="item in agua" :key="item.id">
                    <div class="row" v-if="item.id <= idmin"><span class="col"><strong>{{ item.nome }}</strong></span> <span class="col"><img :src="'/icons/' + item.img"></span> <span class="col"><button type="button" class="btn-agua" @click="escolher(item)">Escolher</button><br/><button type="button" class="btn-ver" @click="vermon(item)">Ver</button></span></div>
                </div>
            </div>
        </div>
        <div v-if="vitorias >= 6" class="row" :class="[{ bckgrnd: !esconder },{ bckgrnd: !esconder_escolher }]">
            <div class="col" v-for="item in extras" :key="item.id">
                <div class="row"><span class="col"><strong>{{ item.nome }}</strong></span> <span class="col"><img :src="'/icons/' + item.img"></span> <span class="col"><button type="button" class="btn-extras" @click="escolher(item)">Escolher</button><br/><button type="button" class="btn-ver" @click="vermon(item)">Ver</button></span></div>
            </div>
        </div>
        
    </div>
</template>

<script>
export default {
    name: 'Listar',
    data(){
        return{
            mons: null,
            grama: null,
            fogo: null,
            agua: null,
            extras: null,
            mon: {
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
            esconder: true,
            esconder_escolher: true,
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
            idmin: 8,
        }
    }, props:{
        vitorias: Number,
    },
    methods: {
        // Pop up da opção ver
        vermon(item){
            this.mon = item
            this.esconder = false
        },
        async getMons(){
            const req = await fetch('http://localhost:3000/mons');
            const data = await req.json();

            this.grama = data[0].grama
            this.fogo = data[1].fogo
            this.agua = data[2].agua
            this.extras = data[3].extra
        },
        async getChars(){
            const req = await fetch('http://localhost:3000/time');
            const data = await req.json();

            this.p1 = data[0].p
            this.p2 = data[1].p
            this.p3 = data[2].p
        },
        escolher(item){
            this.esconder_escolher = false
            this.mon = item
        },
        escolherp1(m){
            this.p1 = m
            setTimeout(()=>{this.esconder_escolher = true
                this.setChar(1)
                this.$emit('page1')
            },500)
        },
        escolherp2(m){
            this.p2 = m
            setTimeout(()=>{this.esconder_escolher = true
                this.setChar(2)
                this.$emit('page1')
            },500)
        },
        escolherp3(m){
            this.p3 = m
            setTimeout(()=>{this.esconder_escolher = true
                this.setChar(3)
                this.$emit('page1')
            },500)
        },
        async setChar(a){
            var data;

            if (a == 1){
                data = {"id": a, "p": this.p1}
            } else if (a == 2){
                data = {"id": a, "p": this.p2}
            } else if (a == 3){
                data = {"id": a, "p": this.p3}
            }

            const datajson = JSON.stringify(data);

            const req = await fetch(`http://localhost:3000/time/${a}`,{
                method: "PUT",
                headers: {"content-type":"application/json"},
                body: datajson
            })

            const res = await req.json();
            a = res
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
        this.getChars();
        this.checkmin();
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
.btn-ver {
    background-color: gray;
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
</style>