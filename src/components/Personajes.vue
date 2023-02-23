<script>
import axios from 'axios'

let API_URL = 'https://rickandmortyapi.com/api/character'

export default {

  data() {
    return {
      info: [],
      personajes: [],
      cont:2

    }
  },

  mounted() {
    axios.get(API_URL)
      .then((response) => {
        this.info = response.data.info;
        this.personajes = response.data.results;
      })
  },

  methods: {
    //metodo para pagina anterior
    pagRe(num) {
      API_URL='https://rickandmortyapi.com/api/character/?page='+(num-1)
      console.log(API_URL)
      axios.get(API_URL)
      .then((response) => {
        console.log(response.config)
        this.info = response.data.info;
        console.log(this.info)
        this.personajes = response.data.results;
      })
      this.cont = this.cont-1
      
      
    },
    //metodo para pagina siguiente
    pag(num) {
      API_URL='https://rickandmortyapi.com/api/character/?page='+num
      console.log(API_URL)
      axios.get(API_URL)
      .then((response) => {
        console.log(response.config)
        this.info = response.data.info;
        console.log(this.info)
        this.personajes = response.data.results;
      })
      this.cont++
    }
  },

}

</script>

<template>

  <h2>Hay un total de {{ info.count }} personajes en el programa de Rick & Morty</h2>
  
  <ul>
    <li v-for="p in personajes">
      <a href="./Personaje.vue" >{{ p.name }} id:{{ p.id }}</a> 
    </li>
  </ul>

  
  <button @click="pagRe(cont)">Anterior </button>
  <button @click="pag(cont)">{{ cont }}</button>
  <button @click="pag(cont)">Siguiente </button>

</template>