<script setup>
defineProps({
  msg: {
    type: String,
    required: true
  }
})
</script>

<template>
  <div class="w-full top-0 left-0 flex-col  px-2 gap-5">

    <h1 class="text-white text-center m-5 bg-gradient-to-r from-green-400 to-pink-500">{{ msg }}</h1>
    
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

</style>
