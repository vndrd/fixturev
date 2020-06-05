<script>
export default {
    name:  'Table'    ,
    data(){
    return {
        allClubs : [
            {id:1, cntVisita: 0,cntLocal:0, last: ''},
            {id:2, cntVisita: 0,cntLocal:0, last: ''},
            {id:3, cntVisita: 0,cntLocal:0, last: ''},
            {id:4, cntVisita: 0,cntLocal:0, last: ''},
            {id:5, cntVisita: 0,cntLocal:0, last: ''},
            {id:6, cntVisita: 0,cntLocal:0, last: ''},
            {id:7, cntVisita: 0,cntLocal:0, last: ''},
            {id:8, cntVisita: 0,cntLocal:0, last: ''},
            {id:9, cntVisita: 0,cntLocal:0, last: ''},
            {id:10, cntVisita: 0,cntLocal:0, last: ''},
            {id:11, cntVisita: 0,cntLocal:0, last: ''},
            {id:12, cntVisita: 0,cntLocal:0, last: ''},

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
            let half_length = Math.ceil(this.allClubs.length/2),
            result = {
                    right: this.allClubs.slice(half_length),
                left: this.allClubs.slice(0,half_length),
            }
            console.clear()
            for (let i = 0; i < this.numberClubs-1;i++) {
                let jornada = {
                    numero: i+1,
                    
                    partidosObjeto: [],
                }
                for (let j = 0; j < half_length; j++) {
                    let local = i % 2 == 1 ? result.left[j].id: result.right[j].id
                    let visita = i % 2 == 0 ? result.left[j].id: result.right[j].id
                    jornada.partidosObjeto.push( {
                        local: local ,
                        visita: visita  ,
                    })
                }
                let [first_izq] = result.left.splice( 1,1 )    ,
                    [last_der] = result.right.splice( -1,1 )
                result.right.unshift(first_izq)
                result.left.push(last_der)
                this.matches.push(jornada)
            }
            this.matches.map( (jornada) => {
                jornada.partidosObjeto.map( ({local,visita}) => {
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
        },
    },
}
</script>

<template>
<div>
    <div class="jornada" v-for="(jornada,index) in matches" :key="index">
        <h6>asd</h6>
        <ul>
            <li v-for="(partido,i) in jornada.partidosObjeto" :key="i+'q'">
                {{partido.local}} - {{partido.visita}}
            </li>
        </ul>
    </div>








    <form>
        <input type="text" v-model="name">
        <br>
        <button @click="agregar"> Agregar </button>
        <br>
    </form>
    <br>
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