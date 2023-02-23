<script setup>
import { RouterLink, RouterView } from "vue-router";
import Titulo from "./components/Titulo.vue";
</script>

<template class="bg-purple-200 w-full h-full">

    <div class="flex-col w-full bg-purple-100">
      <nav>
        <RouterLink to="/">Inicio</RouterLink>
        <RouterLink to="/personajes">Personajes</RouterLink>
        <RouterLink to="/about" class="py-5">Acerca de</RouterLink>
      </nav>

    <header>
      
      <div class="">

        <Titulo msg="Directorio de personajes de Rick & Morty" />

        
      </div>

    </header>


  <RouterView />
</div>
</template>

