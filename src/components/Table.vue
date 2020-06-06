<script>
export default {
    name:  'Table'    ,
    data(){
    return {
        observado: 0,
        numEquipos: '',
        allClubs : [
            {id:1, cntVisita: 0,cntLocal:0, last: '',nombre: ''},
            {id:2, cntVisita: 0,cntLocal:0, last: '',nombre: ''},
            {id:3, cntVisita: 0,cntLocal:0, last: '',nombre: ''},
            {id:4, cntVisita: 0,cntLocal:0, last: '',nombre: ''},
            {id:5, cntVisita: 0,cntLocal:0, last: '',nombre: ''},
            {id:6, cntVisita: 0,cntLocal:0, last: '',nombre: ''},
            {id:7, cntVisita: 0,cntLocal:0, last: '',nombre: ''},
            {id:8, cntVisita: 0,cntLocal:0, last: '',nombre: ''},
            {id:9, cntVisita: 0,cntLocal:0, last: '',nombre: ''},
            {id:10, cntVisita: 0,cntLocal:0, last: '',nombre: ''},
            {id:11, cntVisita: 0,cntLocal:0, last: '',nombre: ''},
            {id:12, cntVisita: 0,cntLocal:0, last: '',nombre: ''},
        ],
        matches: [],
        name: ''
        }
    },
    mounted(){
        this.miAlgoritmo()
    },
    methods: {
        agregar(e){
            e.preventDefault()
            this.name=''
        },
        miAlgoritmo() {
            let dividirGrupos = () => {
                let mitad_tam = Math.ceil(this.allClubs.length/2)
                return {
                    derecha: this.allClubs.slice(mitad_tam),
                    izquierda: this.allClubs.slice(0,mitad_tam),
                }
            }
            let cambiarLocalia = (i,der,izq) => ({
                local: i % 2 == 1 ? izq.id: der.id,
                visita: i % 2 == 0 ? izq.id: der.id
            })
            let {derecha, izquierda} = dividirGrupos()
            for (let i = 0; i < this.allClubs.length-1;i++) {
                let jornada = {
                    numero: i+1,
                    partidos: [],
                }
                for (let j = 0; j < this.allClubs.length/2; j++) {
                    let {local, visita} = cambiarLocalia( i, derecha[j],izquierda[j])
                    jornada.partidos.push( {
                        local, visita
                    })
                }
                //intercambiar items
                let [second_izq] = izquierda.splice( 1,1 )    ,
                    [last_der] = derecha.splice( -1,1 )
                derecha.unshift(second_izq)
                izquierda.push(last_der)
                this.matches.push(jornada)
            }
        },
        verificarBalanceados: function(){
            this.matches.map( (jornada) => {
                jornada.partidos.map( ({local,visita}) => {
                    this.allClubs.map( club => {
                        if( club.id === local ) {
                            club.cntLocal++;
                        }else if( club.id === visita ){
                            club.cntVisita++;
                        }
                    } )
                }
                )
            })
        }
    },
}
</script>

<template>
<div>
    <b-container class="mb-3" >
        <b-row>
            <b-col md="4" class="formulario mb-3" >
                <div  class="p-5">
                    <label for="text-password" class="text-light">Número de equipos</label>
                    <b-input type="text" v-model="numEquipos" class="text-center"></b-input>
                    <b-form-text class="text-light">
                        Número de equipos par, mayor a 2.
                    </b-form-text>
                    <b-button variant="success" @click="agregar" class="mt-3">Agregar</b-button>
                </div>
            </b-col>
            <b-col md="8" class="mb-3">
                <div class="p-5" style="background:gray;">
                    <b-row>
                        <b-col md="4" class="mb-4">
                            <label for="text-password" class="text-light float-left">Agregar equipo</label>
                            <b-input type="text" v-model="nombre"></b-input>
                            <b-button variant="info" @click="agregar" class="mt-3 float-right">Agregar</b-button>
                        </b-col>
                        <b-col md="8" class="mb-4">
                            <b-list-group>
                                <b-list-group-item variant="info">Clubes</b-list-group-item>
                                <b-list-group-item>Dapibus ac facilisis in</b-list-group-item>
                                <b-list-group-item>Morbi leo risus</b-list-group-item>
                                <b-list-group-item>Porta ac consectetur ac</b-list-group-item>
                                <b-list-group-item>Vestibulum at eros</b-list-group-item>
                            </b-list-group>
                        </b-col>
                    </b-row>
                </div>
            </b-col>
        </b-row>
    </b-container>
        <!-- <select v-model="observado">
            <option v-for="club in allClubs" :key="club.id" :value="club.id">{{club.id}}</option>
        </select> -->
    <b-container>
        <b-row>
            <b-col sm="6" md="2" v-for="(jornada,index) in matches" :key="index" >
                <b-card bg-variant="dark" text-variant="white" :header="'Jornada '+jornada.numero" class="text-center">
                    <p v-for="(partido,i) in jornada.partidos" :key="i+'q'">
                        <span :style="[observado == partido.local ? {'background':'lightgreen'}:{} ]">
                            {{partido.local}}
                        </span>
                        -
                        <span :style="[observado == partido.visita ? {'background':'orange'}:{} ]">
                            {{partido.visita}}
                        </span>
                    </p>
                </b-card>
            </b-col>
        </b-row>
    </b-container>








    
    <!-- <table style="border:1px solid;">
    <thead>
        <tr>
        <th>teams</th>
        <th v-for="(club,i) in allClubs" :key="i"> {{club}}</th>
        </tr>
    </thead>
    <tbody>
        <tr v-for="(club,itemClub) in allClubs" :key="itemClub">
        <th>{{club}}</th>
        <td v-for="(club,item) in allClubs.length" :key="item">
            {{itemClub==item?'':'qwe'}}
        </td>
        </tr>
    </tbody>
    </table> -->
</div>
</template>
<style lang="scss" scoped>
.card {
    margin-bottom: 1rem;
}
.card-body {
    background-color: rgba(100, 100, 100, 0.69);;
}
.color-verde{
    background: lightgreen;
}
.color-rojo{
    background: orangered;
}
.formulario {
    background: rgba($color: #4c5e70, $alpha: 1);
}
</style>