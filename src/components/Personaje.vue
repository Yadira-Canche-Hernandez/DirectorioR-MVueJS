<script>
import axios from 'axios'

let API_URL = 'https://rickandmortyapi.com/api/character/?id='

export default {

  data() {
    return {
      info: [],
      personajes: [],
      cont:2

    }
  },

  mounted() {
    axios.post(API_URL)
      .then((response) => {
        this.info = response.data.info;
        this.personajes = response.data.results.id;
      })
  },

  methods: {
    //metodo para pagina anterior
    pagRe(num) {
      API_URL='https://rickandmortyapi.com/api/character/?id='+(num-1)
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
      API_URL='https://rickandmortyapi.com/api/character/?id='+num
      console.log(API_URL)
      axios.get(API_URL)
      .then((response) => {
        console.log(response.config.info.id)
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

  <h2>{{ personajes.name }} </h2>

  <button @click="pagRe(cont)">Anterior </button>
  <button @click="pag(cont)">{{ cont }}</button>
  <button @click="pag(cont)">Siguiente </button>

</template>