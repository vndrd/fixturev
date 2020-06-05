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
                    partidos: [],
                    partidosObjeto: [],
                }
                // let first = result.left[0]
                for (let j = 0; j < half_length; j++) {
                    let local = j % 2 == 1 ? result.left[j].id: result.right[j].id
                    let visita = j % 2 == 0 ? result.left[j].id: result.right[j].id
                    // console.log(`${ result.left[j].id } - ${ result.right[j].id }`);
                    jornada.partidos.push(`
                        ${ local } - ${ visita }
                    `)
                    jornada.partidosObjeto.push( {
                        local: local ,
                        visita: visita  ,
                    })
                }
                let [izq] = result.left.splice( 1,1 )    ,
                    [der] = result.right.splice( -1,1 )
                result.right.unshift(izq)
                result.left.push(der)

                this.matches.push(jornada)
            }
            this.matches.map( ({numero, partidos}) => {
                console.log({numero})
                console.table( partidos)
            })
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
            this.allClubs.map( ({id,cntLocal,cntVisita}) => {
                console.table({id,cntLocal,cntVisita})
            })
        },
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
    </form>
    <br>
    <table style="border:1px solid;">
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
    </table>
</div>
</template>