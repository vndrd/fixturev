<script>
export default {
    name:  'Table'    ,
    data(){
    return {
        observado: 0,
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
        numberClubs: 12,
        name: ''
        }
    },
    mounted(){
        this.miAlgoritmo()
    },
    methods: {
        agregar(e){
            e.preventDefault()
            this.allClubs.push(this.name)
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
            for (let i = 0; i < this.numberClubs-1;i++) {
                let jornada = {
                    numero: i+1,
                    partidos: [],
                }
                for (let j = 0; j < this.numberClubs/2; j++) {
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
    <form>
        <input type="text" v-model="name">
        <br>
        <button @click="agregar"> Agregar </button>
        <br>
        <select v-model="observado">
            <option v-for="club in allClubs" :key="club.id" :value="club.id">{{club.id}}</option>
        </select>
    </form>
    <br>
    <div class="jornada" v-for="(jornada,index) in matches" :key="index">
        <h6>asd</h6>
        <ul>
            <li v-for="(partido,i) in jornada.partidos" :key="i+'q'">
                <span :style="[observado == partido.local ? {'background':'red'}:{} ]">
                    {{partido.local}}
                </span>
                -
                <span :style="[observado == partido.visita ? {'background':'red'}:{} ]">

                  {{partido.visita}}
                </span>
            </li>
        </ul>
    </div>








    
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