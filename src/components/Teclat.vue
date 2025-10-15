<template>
  <div id="keyboard">
    <div 
      v-for="(linea, index) in filesDelTeclat"
      :key = index
      class = "filaTeclat"
    >
      <button
        v-for="(lletra, indexLletra) in linea"
        :key = indexLletra
        :class = "{ 'tecla-premuda': teclaPremuda == filesDelTeclat[index][indexLletra]}"
      >{{ lletra }}</button>
    </div>
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

button{
  align-items: center;
  appearance: none;
  background-color: #FCFCFD;
  border-radius: 4px;
  border-width: 0;
  box-shadow: rgba(45, 35, 66, 0.4) 0 2px 4px,rgba(45, 35, 66, 0.3) 0 7px 13px -3px,#D6D6E7 0 -3px 0 inset;
  box-sizing: border-box;
  color: #36395A;
  cursor: pointer;
  display: inline-flex;
  font-family: "comic",monospace;
  height: 48px;
  justify-content: center;
  line-height: 1;
  list-style: none;
  overflow: hidden;
  padding-left: 16px;
  padding-right: 16px;
  position: relative;
  text-align: left;
  text-decoration: none;
  transition: box-shadow .15s,transform .15s;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  white-space: nowrap;
  will-change: box-shadow,transform;
  font-size: 18px;
  margin: 4px;
}

.tecla-premuda{
  box-shadow: #D6D6E7 0 6px 14px inset;
  transform: translateY(3px);
}

#keyboard{
  max-width: fit-content;
  margin: auto; 
  background-color: #fdfdfe;
  padding: 20px 10px 35px 10px;
  box-shadow: rgba(45, 35, 66, 0.4) 0 2px 4px,rgba(45, 35, 66, 0.3) 0 7px 13px -3px,#D6D6E7 0 -10px 0 inset;
  border-radius: 3%;
}
</style>