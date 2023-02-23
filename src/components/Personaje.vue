<script>
import axios from 'axios'



let API_URL = 'https://rickandmortyapi.com/api/character/${id}'
console.log(API_URL)

export default {
    props: ['id'],
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

  } 
}
</script>

<template>
    <div >
        <h2>Nombre: {{ p.name }} </h2>
    </div>
</template>