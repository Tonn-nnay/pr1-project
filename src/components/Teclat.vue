<template>
  
    <div 
      v-for="(linea, index) in filesDelTeclat"
      :key = index
    >
      <button
        v-for="(lletra, indexLletra) in linea"
        :key = indexLletra
        :class = "{ 'tecla-premuda': teclaPremuda == filesDelTeclat[index][indexLletra]}"
      >{{ lletra }}</button>
    </div>
</template>

<script setup>
  import { ref, onMounted, onUnmounted } from 'vue';

  const filesDelTeclat = ref([
      ['Q', 'W', 'E', 'R', 'T', 'Y', 'U', 'I', 'O', 'P'],
      ['A', 'S', 'D', 'F', 'G', 'H', 'J', 'K', 'L'],
      ['Z', 'X', 'C', 'V', 'B', 'N', 'M']
  ]);
  const teclaPremuda = ref('');

  function handleKeyDown(event){
    teclaPremuda.value = event.key.toUpperCase()
    setTimeout(() => {
      teclaPremuda.value = '';
    } , 200);
  }

  //En onMounted i en onUnmounted, el AEL no truca cap funció, pasa una referencia i llavor s'executa
  onMounted(() => {
    window.addEventListener('keydown', handleKeyDown)
  })

  onUnmounted(() => {
    window.removeEventListener('keydown', handleKeyDown)
  })

  
</script>

<style>
  .tecla-premuda{
    background-color:aqua
  }
</style>