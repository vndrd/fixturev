<script>
export default {
    name:  'Table'    ,
    data(){
        return {
            generarClicked: false,
            numEquipos: '',
            allTeams: [],
            matches: [],
            nombre: '',
            observado: 0,
        }   
    },
    mounted(){
    
    },
    methods: {
        agregar(e){
            e.preventDefault()
            
        },
        rellenar(){
            for (let i = 0; i < this.numEquipos; i++) {
                this.allTeams.push({
                    name: '',
                    id: i+1,
                    edit: false,
                })                
            }
        },
        generar(e){
            e.preventDefault()
            
            if ( this.numEquipos < 4 ){
                alert("Introducir número mayor o igual a 4")
                return 
            }
            if ( this.numEquipos %2 === 1 ){
                alert("Introducir número par")
                return  
            }
            this.rellenar()
            this.miAlgoritmo()

            this.generarClicked = true
        },
        miAlgoritmo() {
            let dividirGrupos = () => {
                let mitad_tam = Math.ceil(this.numEquipos/2)
                return {
                    derecha: this.allTeams.slice(mitad_tam),
                    izquierda: this.allTeams.slice(0,mitad_tam),
                }
            }
            let cambiarLocalia = (i,der,izq) => ({
                local: i % 2 == 1 ? izq.id: der.id,
                visita: i % 2 == 0 ? izq.id: der.id
            })
            let {derecha, izquierda} = dividirGrupos()
            for (let i = 0; i < this.numEquipos-1;i++) {
                let jornada = {
                    numero: i+1,
                    partidos: [],
                }
                for (let j = 0; j < this.numEquipos/2; j++) {
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
                    this.allTeams.map( club => {
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
    computed: {
        nroEquiposRestantes: function() { return this.numEquipos }
    }
}
</script>
<template>
<div>
    <b-container class="mb-3" >
        <b-row>
            <b-col md="4" class="mb-3" >
                <div class="formulario mb-3">
                    <div class="header-title p-3">
                        <h2>Fixture aleatorio</h2>
                    </div>
                    <div  class="p-5">
                        <label for="text-password" class="text-light">Número de equipos</label>
                        <b-input type="text" v-model="numEquipos" :disabled="generarClicked" class="text-center"></b-input>
                        <b-form-text class="text-light" >
                            Número de equipos par, mayor a 2.
                        </b-form-text>
                        <b-button variant="success" @click="generar" class="mt-3" :disabled="generarClicked">
                            Generar</b-button>
                    </div>    
                </div>
                <b-card no-body header="Equipos" v-if="generarClicked" bg-variant="secondary" text-variant="light">    
                    <b-list-group-item v-for="(team,i) in allTeams" :key="i" 
                        @dblclick="team.edit=true" 
                        @blur="team.edit=false; $emit('update')"
                    @keyup.enter="team.edit=false; $emit('update')">
                        <b-badge  variant="warning" class="float-left counter">    {{i+1}}</b-badge>                        
                        <p v-if="team.edit==false" :class="team.name=='' ? 'italic':''">
                            {{` ${team.name=='' ?'Equipo '+(i+1): team.name }`}}
                        </p>
                        <input type="text" v-if="team.edit==true" v-model="team.name">
                    </b-list-group-item>
                    <b-list-group-item variant="secondary">
                    </b-list-group-item>
                </b-card>
            </b-col>
        <!-- <select v-model="observado">
            <option v-for="club in allTeams" :key="club.id" :value="club.id">{{club.id}}</option>
        </select> -->
            <b-col md="8" v-if="generarClicked">
                <b-row>
                    <b-col sm="6" md="4" v-for="(jornada,index) in matches" :key="index" >
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
            </b-col>            
        </b-row>
    </b-container>
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
.list-group-item {
    background-color: rgba($color: #aaa, $alpha: 1  );
    font-size: 1.2rem;
}
.counter {
    width: 2rem;
    display: inline-block;
    vertical-align: middle;
    line-height: normal;
    height: 2rem;
    font-size: 1.2rem;
    color: white;
}
.header-title{
    position:relative;
    width: calc(100% + 30px);
    transform: translateX(-15px);
    background-color: darkcyan;
    h2{
        font-weight: 100;
        width: 100%;
        color: white;
    }
}
.italic {
    font-size: 1rem;
    font-style: italic;
    color: #ddd;
}
</style>