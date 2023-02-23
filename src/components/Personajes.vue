<script>
import axios from 'axios'


let API_URL = 'https://rickandmortyapi.com/api/character'

export default {

  data() {
    return {
      info: [],
      personajes: [],
      cont:2,
      search:"",
      id: 0,

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
        this.prev = this.info.prev;
        
      })
      this.cont = this.cont-1
      
      
    },
    //metodo para pagina siguiente
    pagSig(num) {
      //url de consumo API
      API_URL='https://rickandmortyapi.com/api/character/?page='+num 
      //lo obtiene
      axios.get(API_URL)
      //
      .then((response) => {
        
        this.info = response.data.info;
        this.personajes = response.data.results;
      })
      this.cont++
    },
    InfoPer(id) {
      //url de consumo API
      API_URL='https://rickandmortyapi.com/api/character/?id='+id 
      //lo obtiene
      axios.get(API_URL)
      //
      .then((response) => {
        this.info = response.data.info;
        this.personajes = response.data.results.id;
        console.log(this.personajes)
      })
      this.id++
    }
  },
  computed: {
    //metodo para buscar
    filtrarBusqueda(){
      //busca en cada personaje
      return this.personajes.filter((personaje) => {
        //busca en el nombre del personaje
        return personaje.name.toLowerCase().includes(this.search.toLowerCase())
      })
    },

  },
}

</script>

<template class="flex mx-auto">

  <!--Total de personajes-->
  <h2 class="text-2xl my-10 text-center mx-5 sm:py-2">Hay un total de {{ info.count }} personajes en el programa de Rick & Morty</h2>

  <!--buscador por nombre por pagina-->
  <div class="flex justify-align-center mx-auto  my-5 ">
      
    <h3 class="text-2xl text-center my-3 mx-5 sm:py-2">Buscar</h3>
    <input type="text" v-model="search" placeholder="Buscar por nombre">
  </div>


  <div class="content-personajes" v-for="personaje in filtrarBusqueda"  :key="personaje.id">

    <div class="personaje">
      <img :src="personaje.image" alt="">
      <div class="info-personaje">
        <h2> {{ personaje.name}}</h2>
        <h4> {{ personaje.species}}</h4>
        <h4> {{ personaje.status}}</h4>
      </div>
    </div>

  </div>


  <!--Listado de cada 20 personajes o sea pagina-->
  <div class="text-2xl my-10 text-black mx-5 sm:py-2">
    <ul class="text-xl">
      <li v-for="p in personajes">
        <button @click="InfoPer(id)">{{ p.name }} id:{{ p.id }}</button>
        <!--<RouterLink to="/personaje" class="py-5" :id="p.id"> {{ p.name }} </RouterLink> -->
      </li>
    </ul>
  </div>
    
  <!--Botones paginas-->
  <div class="mt-5">
      <!--Pagina anterior-->
      <button @click="pagRe(cont)" class="h-14 sm:h-10 px-7 font-semibold rounded-md bg-slate-800 text-white mb-8 justify-center shadow-xl hover:shadow-inner transition duration-500 ease-in-out  transform hover:-translate-x hover:scale-105" type="center">Anterior </button>
      <!--Contador-->
      <button @click="pag(cont)" class="h-14 sm:h-10 px-7 font-semibold rounded-md bg-slate-800 text-white mb-8 justify-center shadow-xl hover:shadow-inner transition duration-500 ease-in-out  transform hover:-translate-x hover:scale-105" type="center">{{ cont }}</button>
      <!--Pagina anterior-->
      <button @click="pagSig(cont)" class="h-14 sm:h-10 px-7 font-semibold rounded-md bg-slate-800 text-white mb-8 justify-center shadow-xl hover:shadow-inner transition duration-500 ease-in-out  transform hover:-translate-x hover:scale-105" type="center">Siguiente </button>
  </div>
  
</template>