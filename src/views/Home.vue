<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
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
          <th v-for="club in allClubs" :key="club"> {{club}}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="club in allClubs" :key="club">
          <td>{{club}}</td>
          <td v-for="(club,item) in allClubs.length-1" :key="item">qwe</td>

        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'Home',
  data(){
    return {
      allClubs : ['Bolivar','The Strongest','Always Ready','Wilstermann'],
      name: ''
    }
  },
  methods: {
    agregar(){
      [this.allClubs] = [...this.allClubs, name]
      this.name=''
    },
  },
  components: {
  }
}
</script>
