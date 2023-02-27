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
      mostrarListas: true,
      cont:2,

      //para tarjeta por personaje desde lista
      id: 0,
      infoUno:[],
      personaje: [],
      mostrar: false,

      //para buscador
      buscar:"",
      buscado:[],
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
    pagAnte() {
     //lo obtiene de info guardado en mounted llamando a prev
     axios.get(this.info.prev)
      .then((response) => {
        this.info = response.data.info;
        this.personajes = response.data.results;
      })
      if (this.cont < 3){
        this.cont == this.cont
      }
      else{
        this.cont-1
      }
    },

    //metodo para pagina siguiente
    pagSig() {  
      //lo obtiene de info guardado en mounted llamando a next
      axios.get(this.info.next)
      .then((response) => {
        
        this.info = response.data.info;
        this.personajes = response.data.results;
      })
      if (this.cont > 41){
        this.cont == this.cont
      }
      else{
        this.cont++
      }
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
      if(buscar==""){
        this.mostrarListas= true
        this.mostrarBuscados = false
        this.mostrarBuscadoID = false
      }
      else{
        this.mostrarListas= false
        console.log(!isNaN (buscar)) //comprobando que funcione con la funcion isNan
        //si lo que recibe del input es un numero
        if(!isNaN (buscar) === true) {
          //busca al personaje por su id
          API_URL='https://rickandmortyapi.com/api/character/'+buscar
          axios.get(API_URL) //usando axios
          .then((response) => {
            //toma todos los personajes que encuentra en resultados y lo almacena en la variable buscados
            console.log(response.data)
            this.buscado = response.data;
            console.log(this.buscado)
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
          //devolvemos falso por id
          this.mostrarBuscadoID = false
          //cambiamos a verdadero que recibe por nombre 
          this.mostrarBuscados = true
          
        }
          console.log(API_URL) //comprobando API_URL en consola
      }
    },
  }
}

</script>

<template class="flex mx-auto">

  <!--Total de personajes-->
  <h2 class="text-2xl my-10 text-center mx-5 sm:py-2">Hay un total de {{ info.count }} personajes en el programa de Rick & Morty</h2>

  <!--Buscador-->
  <div class="flex justify-center items-center space-x-4 text-base my-2">
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
  <div v-if="mostrarBuscadoID" class="flex mx-auto justify-center items-center space-x-4 text-base my-2">
    <!--Recorre la nueva lista y por cada personaje que encuentra-->    
    <div class="w-1/4 mx-auto my-auto p-3 border-4 border-black bg-white rounded-sm">
      <!--muestra en pantalla la carta de personaje-->
      <!--carta de personaje-->          
      <img :src="buscado.image" alt="" class="pb-4 drop-shadow-xl rounded-md">  
        <h2> <b>Id: </b> {{ buscado.id}}</h2>
        <h2> <b>Nombre:</b> {{ buscado.name}}</h2>
        <h4> <b>Especie:</b> {{ buscado.species}}</h4>
        <h4> <b>Estado:</b> {{ buscado.status}}</h4>
        <h4> <b>Tipo:</b> {{ buscado.type}}</h4>
        <h4> <b>Locación:</b> {{ buscado.location.name}}</h4>
    </div>      
  </div>

  <!--si no recibe true en la variable mostrarBuscados
      ENCONTRO VARIOS PERSONAJES POR SU NOMBRE-->
  <div class="">
  <div v-if="mostrarBuscados" class="grid grid-cols-3 divide-x-0.5 justify-center items-center text-base my-2 p-3 mx-2">
   
    <!--Recorre la nueva lista y por cada personaje que encuentra-->    
    <div v-for="buscado in buscados" class=" my-5 mx-auto p-3 border-4 border-black bg-white rounded-sm">
      
      <!--carta de personaje-->          
        <img :src="buscado.image" alt="" class="pb-4 drop-shadow-xl rounded-md">  
        <h2> <b>Id: </b> {{ buscado.id}}</h2>
        <h2> <b>Nombre:</b> {{ buscado.name}}</h2>
        <h4> <b>Especie:</b> {{ buscado.species}}</h4>
        <h4> <b>Estado:</b> {{ buscado.status}}</h4>
        <h4> <b>Tipo:</b> {{ buscado.type}}</h4>
        <h4> <b>Locación:</b> {{ buscado.location.name}}</h4>
      
    </div>    
  </div>
</div>

  <div v-if="mostrarListas">
  <!--Botones paginas-->
  <div class="flex justify-center items-center space-x-4 text-base my-8">
      <!--Pagina anterior-->
      <button @click="pagAnte()" class="h-14 mx-2 sm:h-10 px-7 font-semibold rounded-md bg-slate-800 text-white my-2 justify-center shadow-xl hover:shadow-inner transition duration-500 ease-in-out  transform hover:-translate-x hover:scale-105" type="center">Anterior </button>
      <!--Pagina siguiente-->
      <button @click="pagSig(cont)" class="h-14 mx-2 sm:h-10 px-7 font-semibold rounded-md bg-slate-800 text-white my-2 justify-center shadow-xl hover:shadow-inner transition duration-500 ease-in-out  transform hover:-translate-x hover:scale-105" type="center">Siguiente </button>
  </div>
  
  <div class="grid grid-cols-2 ">
    <div class="m-auto" >
      <!--Listado de cada 20 personajes o sea pagina-->
      <div class="text-2xl my-10 text-black mx-5 sm:py-2" >
        <h1 > Personajes por página</h1><br>
        <ul class="text-lg">
          <li v-for="p in personajes">
            <!--Llamando función obtener info personal por personaje-->
            <button @click="InfoPer(p.id)"> {{ p.id }}. {{ p.name }} </button> 
          </li>
        </ul>
      </div>
    </div>

    <div class="m-auto"  v-if="mostrar">
      <!--carta de personaje-->          
      <div class="w-3/4 mx-auto my-auto p-3 border-4 border-black bg-white rounded-sm">
        <img :src="infoUno.image" alt="" class="pb-4 drop-shadow-xl rounded-md">  
        <h2> <b>Id: </b> {{ infoUno.id}}</h2>
        <h2> <b>Nombre:</b> {{ infoUno.name}}</h2>
        <h4> <b>Especie:</b> {{ infoUno.species}}</h4>
        <h4> <b>Estado:</b> {{ infoUno.status}}</h4>
        <h4> <b>Tipo:</b> {{ infoUno.type}}</h4>
        <h4> <b>Locación:</b> {{ infoUno.location.name}}</h4>
      </div>
          
    </div>
  
  </div>
</div>
</template>