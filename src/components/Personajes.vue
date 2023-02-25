<script>
import axios from 'axios'


let API_URL = 'https://rickandmortyapi.com/api/character'

export default {

  data() {
    return {
      //variables de retorno  
      //para lista de personajes
      info: [],
      personajes: [],
      cont:2,

      //para tarjeta por personaje desde lista
      id: 0,
      infoUno:[],
      personaje: [],
      mostrar: false,

      //para buscador
      buscar:"",
      buscados: [],
      mostrarBuscadoID: false,
      mostrarBuscados: false,

    }
  },

  mounted() {
    axios.get(API_URL)
      .then((response) => {
        //paginas
        this.info = response.data.info;
        //resultados
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

    //informacion de un personaje
    InfoPer(id) {      
      //url de consumo API
      API_URL='https://rickandmortyapi.com/api/character/'+id 
      console.log(API_URL)
      //lo obtiene
      axios.get(API_URL)
      //tomamos la respuesta
      .then((response) => {
        console.log(response) //regresa datos de un solo personaje
        //contiene el array de cada personaje con sus datos
        this.infoUno = response.data;
        console.log(this.infoUno)
      })
      //el id incrementa
      this.id++
      //cambiamos el valor por verdadero para que lo muestre
      this.mostrar=true
      console.log(this.mostrar)
    },
    //funcion para buscar personajes
    buscador(buscar)  { 
      
        console.log(!isNaN (buscar)) //comprobando que funcione con la funcion isNan

      //si lo que recibe del input es un numero
      if(!isNaN (buscar) === true) {
        //busca al personaje por su id
        API_URL='https://rickandmortyapi.com/api/character/'+buscar
        axios.get(API_URL) //usando axios
        .then((response) => {
          //toma todos los personajes que encuentra en resultados y lo almacena en la variable buscados
          console.log(response.data)
          this.buscados = response.data;
          console.log(this.buscados)
        })
        //si es por ir para que muestre
        this.mostrarBuscadoID = true
        //mantenemos en falso para que no muestre datos de la tarjeta de todos los personajes
        this.mostrarBuscados = false
      }

      //si no es numerico
      else{
        //lo busca por nombre                
        API_URL='https://rickandmortyapi.com/api/character/?name='+buscar
        axios.get(API_URL) //usando axios
        .then((response) => {
          //toma todos los personajes que encuentra en resultados y lo almacena en la variable buscados
          this.buscados = response.data.results;
          console.log(this.buscados)
        })
        //cambiamos a verdadero que recibe por nombre 
        this.mostrarBuscados = true
        //devolvemos falso por id
        this.mostrarBuscadoID = false
      }

      console.log(API_URL) //comprobando API_URL en consola
        
        
    },
  },
}

</script>

<template class="flex mx-auto">

  <!--Total de personajes-->
  <h2 class="text-2xl my-10 text-center mx-5 sm:py-2">Hay un total de {{ info.count }} personajes en el programa de Rick & Morty</h2>

  <!--Buscador-->
  <div class="flex justify-center items-center space-x-4 text-base my-8">
    <input class="h-12 wl-5 px-7" type="text" v-model="buscar" placeholder="Buscar por nombre o id">
    <button
      @click="buscador(buscar)" 
      class="h-10 wl-5 px-7 font-semibold rounded-md bg-black text-white justify-center shadow-xl hover:shadow-inner transition duration-500 ease-in-out  transform hover:-translate-x hover:scale-105" type="center">
      Buscar
    </button>
  </div>
  
  <!--Resultados Buscador-->

  <!--si recibe true en la variable mostrarBuscadosID
      ES QUE BUSCO UN PERSONAJE POR SU ID-->
  <div v-if="mostrarBuscadoID" class="container flex flex-col items-center mx-auto md:flex md:gap-4" >
    <!--Recorre la nueva lista y por cada personaje que encuentra-->    
    <div class="buscado por id ">
      <!--muestra en pantalla la carta de personaje-->
      <img :src="buscados.image" alt="">
      <div class="info-buscados">
        <h2> Id {{ buscados.id}}</h2>
        <h2> Nombre {{ buscados.name}}</h2>
        <h4> Especie {{ buscados.species}}</h4>
        <h4> Estado {{ buscados.status}}</h4>
      </div>
    </div>      
  </div>

  <!--si no recibe true en la variable mostrarBuscados
      ENCONTRO VARIOS PERSONAJES POR SU NOMBRE-->
  <div v-if="mostrarBuscados">
    <!--Recorre la nueva lista y por cada personaje que encuentra-->    
    <div class="buscados" v-for="buscado in buscados">
      <!--muestra en pantalla la carta de personaje-->
      <img :src="buscado.image" alt="">
      <div class="info-buscados">
        <h2> Id {{ buscado.id}}</h2>
        <h2> Nombre {{ buscado.name}}</h2>
        <h4> Especie {{ buscado.species}}</h4>
        <h4> Estado {{ buscado.status}}</h4>
      </div>
    </div>      
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
  
  <div class="grid grid-cols-2">
    <div>
      <!--Listado de cada 20 personajes o sea pagina-->
      <div class="text-2xl my-10 text-black mx-5 sm:py-2">
        <ul class="text-xl">
          <li v-for="p in personajes">
            <!--Llamando función obtener info personal por personaje-->
            <button @click="InfoPer(p.id)">{{ p.name }} id:{{ p.id }}</button> 
          </li>
        </ul>
      </div>
    </div>

    <div>
      <!--carta de personaje-->
      <div v-if="mostrar">
        
        <div class="personaje">

          <img :src="infoUno.image" alt="">
          <div class="info-personaje">
            <h2> Nombre {{ infoUno.name}}</h2>
            <h4> Especie {{ infoUno.species}}</h4>
            <h4> Estado {{ infoUno.status}}</h4>

          </div>
          
        </div>
        
      </div>
    </div>
  </div>

</template>