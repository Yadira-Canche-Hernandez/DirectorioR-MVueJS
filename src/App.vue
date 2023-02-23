<script setup>
import { RouterLink, RouterView } from "vue-router";
import Titulo from "./components/Titulo.vue";
</script>

<template class="bg-purple-200 w-full h-full">

    <div class="flex-col w-full bg-purple-100">
      
      

    <header>

      <nav class ="bg-yellow flex justify-between items-center">

        <RouterLink to="/" class="p-2 text-white block p-5 text-center ">Inicio</RouterLink>
        
        <RouterLink to="/about" class="p-2 text-white block p-5 text-center">¿Quiénes son?</RouterLink>
        
        <RouterLink to="/personajes" class="p-2 text-white block p-5 text-center">Personajes</RouterLink>
        
      </nav>
      
      <div class="w-full">

        <Titulo msg="Directorio de personajes de Rick & Morty" />

        
      </div>

    </header>


  <RouterView />
</div>
</template>

