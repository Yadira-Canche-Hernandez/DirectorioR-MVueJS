<script setup>
import { RouterLink, RouterView } from 'vue-router'
import Titulo from "./components/Titulo.vue";
</script>

<template>

<div class="flex-col w-full bg-purple-200">
      
      

      <header>
  
        <nav class ="bg-black flex justify-self-center items-center text-white gap-5 font-semibold hover:text-gray-700">
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