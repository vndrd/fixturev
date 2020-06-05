<script>
export default {
    name:  'Table'    ,
    data(){
    return {
        allClubs : [
            '1','2','3','4',
            '5','6','7','8',
            '9','10','11','12',
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
            console.log({half_length})
            for (let i = 0; i < this.numberClubs-1;i++) {
                let jornada = []
                // let first = result.left[0]
                for (let j = 0; j < half_length; j++) {
                    jornada.push(`
                        ${ result.left[j] } - ${ result.right[j] }
                    `)
                }
                let [izq] = result.left.splice( 1,1 )    ,
                    [der] = result.right.splice( -1,1 )
                // console.log({first,izq,der})
                result.right.unshift(izq)
                result.left.push(der)
                this.matches.push(jornada)
                console.table(jornada)
            }
            // console.table(this.matches)
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