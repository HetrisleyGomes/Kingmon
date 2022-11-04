<template>
    <div style="overflow-x:hidden; user-select: none;">
        <span >
            <input type="button" v-if="selected_grama == true || selected_fogo == true || selected_agua == true" @click="selected_grama = false; selected_fogo = false; selected_agua = false" class="btn btn-danger" value="Cancelar" style="margin:5px">
            <input type="button" @click="$emit('page3')" class="btn btn-secondary" value="Ignorar" style="margin:5px">
        </span>
        <div class="d-flex justify-content-evenly" style="left:auto; right: auto; margin:auto">
                <div class="card">
                    <img :src="'/icons/' + grama.img" class="card-img-top" alt="..." style="width: 150px; left:auto; right: auto; margin:auto">
                    <div class="card-body">
                        <h4 class="card-title">{{ grama.nome }}</h4>
                        <h6 class="card-subtitle mb-2 text-muted"><strong>[{{ grama.tipo}}]</strong> - {{ grama.classe }} | {{ grama.damage_type }}</h6>
                        <table class="table m-2">
                            <td style="width: 30%;">Ataque {{ grama.atk }}</td>
                            <td style="width: 30%;">Defesa {{ grama.def }}</td>
                            <td style="width: 40%;">Velocidade {{ grama.spd }}</td>
                        </table>
                        <span v-if="selected_grama == false && selected_fogo == false && selected_agua == false">
                            <input type="button" @click="selected_grama = true" class="btn btn-info" value="Escolher">
                        </span>
                    </div>
                </div>
                <div class="card">
                    <img :src="'/icons/' + fogo.img" class="card-img-top" alt="..." style="width: 150px; left:auto; right: auto; margin:auto">
                    <div class="card-body">
                        <h4 class="card-title">{{ fogo.nome }}</h4>
                        <h6 class="card-subtitle mb-2 text-muted"><strong>[{{ fogo.tipo}}]</strong> - {{ fogo.classe }} | {{ fogo.damage_type }}</h6>
                        <table class="table m-2">
                            <td style="width: 30%;">Ataque {{ fogo.atk }}</td>
                            <td style="width: 30%;">Defesa {{ fogo.def }}</td>
                            <td style="width: 40%;">Velocidade {{ fogo.spd }}</td>
                        </table>
                        <span v-if="selected_grama == false && selected_fogo == false && selected_agua == false">
                            <input type="button" @click="selected_fogo = true" class="btn btn-info" value="Escolher">
                        </span>
                    </div>
                </div>
                <div class="card">
                    <img :src="'/icons/' + agua.img" class="card-img-top" alt="..." style="width: 150px; left:auto; right: auto; margin:auto">
                    <div class="card-body">
                        <h4 class="card-title">{{ agua.nome }}</h4>
                        <h6 class="card-subtitle mb-2 text-muted"><strong>[{{ agua.tipo}}]</strong> - {{ agua.classe }} | {{ agua.damage_type }}</h6>
                        <table class="table m-2">
                            <td style="width: 30%;">Ataque {{ agua.atk }}</td>
                            <td style="width: 30%;">Defesa {{ agua.def }}</td>
                            <td style="width: 40%;">Velocidade {{ agua.spd }}</td>
                        </table>
                        <span v-if="selected_grama == false && selected_fogo == false && selected_agua == false">
                            <input type="button" @click="selected_agua = true" class="btn btn-info" value="Escolher">
                        </span>
                    </div>
                </div>
        </div>
        <hr>
        <div class="d-flex justify-content-center" style="left:auto; right: auto; margin:auto">
            <table class="table">
                <tr>
                    <td style="width: 33%;"><h5>{{ p1.nome }}</h5></td>
                    <td style="width: 33%;"><h5>{{ p2.nome }}</h5></td>
                    <td style="width: 33%;"><h5>{{ p3.nome }}</h5></td>
                </tr>
                <tr>
                    <td><img :src="'/icons/' + p1.img" v-if="p1.img != 'placeholder'" alt="..." style="width: 100px; left:auto; right: auto; margin:-20px"></td>
                    <td><img :src="'/icons/' + p2.img" v-if="p2.img != 'placeholder'" alt="..." style="width: 100px; left:auto; right: auto; margin:-20px"></td>
                    <td><img :src="'/icons/' + p3.img" v-if="p3.img != 'placeholder'" alt="..." style="width: 100px; left:auto; right: auto; margin:-20px"></td>
                </tr>
                <tr v-if="selected_grama == true || selected_fogo == true || selected_agua == true">
                    <td><input v-if="p1.img == 'placeholder'" type="button" @click="buyThis(1)" class="btn btn-info" value="Escolher"><input v-else type="button" @click="buyThis(1)" class="btn btn-info" value="Trocar"></td>
                    <td><input v-if="p2.img == 'placeholder'" type="button" @click="buyThis(2)" class="btn btn-info" value="Escolher"><input v-else type="button" @click="buyThis(2)" class="btn btn-info" value="Trocar"></td>
                    <td><input v-if="p3.img == 'placeholder'" type="button" @click="buyThis(3)" class="btn btn-info" value="Escolher"><input v-else type="button" @click="buyThis(3)" class="btn btn-info" value="Trocar"></td>
                </tr>
                <tr v-else>
                    <td>{{ p1.tipo }} <small v-if="p1.img != 'placeholder'"> {{ p1.classe }} | {{ p1.damage_type }}</small></td>
                    <td>{{ p2.tipo }} <small v-if="p2.img != 'placeholder'"> {{ p2.classe }} | {{ p2.damage_type }}</small></td>
                    <td>{{ p3.tipo }} <small v-if="p3.img != 'placeholder'"> {{ p3.classe }} | {{ p3.damage_type }}</small></td>
                </tr>
                <tr>
                    <td><span v-if="p1.img != 'placeholder'">Atk:{{ p1.atk }} | Def:{{ p1.def }} | Spd:{{ p1.spd }}</span></td>
                    <td><span v-if="p2.img != 'placeholder'">Atk:{{ p2.atk }} | Def:{{ p2.def }} | Spd:{{ p2.spd }}</span></td>
                    <td><span v-if="p3.img != 'placeholder'">Atk:{{ p3.atk }} | Def:{{ p3.def }} | Spd:{{ p3.spd }}</span></td>
                </tr>
            </table>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Listar',
    data(){
        return{
            selected_grama: false,
            selected_fogo: false,
            selected_agua: false,
            grama: {
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
            fogo: {
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
            agua: {
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
    }, props: {
        vitorias: Number
    },
    methods: {
        async getMons(){
            const req = await fetch('http://localhost:3000/mons');
            const data = await req.json();
            let a = this.idmin - 1;
            let b = (Math.random()*a).toFixed(0)
            let c = (Math.random()*a).toFixed(0)
            let d = (Math.random()*a).toFixed(0)

            this.grama = data[0].grama[b]
            this.fogo = data[1].fogo[c]
            this.agua = data[2].agua[d]
        },
        async getChars(){
            const req = await fetch('http://localhost:3000/time');
            const data = await req.json();

            this.p1 = data[0].p
            this.p2 = data[1].p
            this.p3 = data[2].p
        }, async buyThis(a){
            var data;

            if (this.selected_grama == true) {
                data = {"p": this.grama}
            }
            if (this.selected_fogo == true) {
                data = {"p": this.fogo}
            }
            if (this.selected_agua == true) {
                data = {"p": this.agua}
            }

            const datajson = JSON.stringify(data);
            await fetch(`http://localhost:3000/time/${a}`,{
                method: "PUT",
                headers: {"content-type":"application/json"},
                body: datajson
            })


            this.getChars();
            this.selected_grama = false;
            this.selected_fogo = false;
            this.selected_agua = false;
            setTimeout(()=>{
                this.$emit('page3')
            },500)
            
        }, checkmin(){
            if(this.vitorias >= 1){
                this.idmin = 9
            }
            if (this.vitorias >= 3){
                this.idmin = 10
            }
        }
        
    },
    mounted() {
        this.checkmin()
        this.getMons();
        this.getChars();
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

span img {
    width: 100px;
}

span {
    text-align: center;
    margin: auto;
}

.imgempty{
    padding-top:263px;
}

.card {
    min-width: 300px;
    padding: 0;
}
</style>