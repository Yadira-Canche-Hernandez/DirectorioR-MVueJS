<script setup>
import { RouterLink, RouterView } from 'vue-router'
import Titulo from "./components/Titulo.vue";
</script>

<template class="flex-col justify-center bg-blue-100 w-full h-full p-2">

<div class="w-full bg-purple-100 justify-items-center">
      
      

      <header>
  
        <nav class ="bg-black flex justify-self-center items-center text-white pl-5 gap-5 font-semibold hover:text-gray-500">
          <RouterLink to="/">Inicio</RouterLink>
          <RouterLink to="/about" class="py-5">¿Quiénes son?</RouterLink>
          <RouterLink to="/personajes">Personajes</RouterLink>
          
        </nav>
        
        <div class="w-full">
  
          <Titulo msg="Directorio de personajes de Rick & Morty" />
  
          
        </div>
  
      </header>
  
  
    <RouterView />
  </div>
  </template>