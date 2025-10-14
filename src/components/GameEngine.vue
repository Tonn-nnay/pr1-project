<template>
  <div class="game-engine">
    <div class="paraules-container">
      <!-- Iterem sobre la llista de paraules -->
      <div 
        v-for="(paraula, index) in estatDelJoc.paraules" 
        :key="paraula.id"
        class="paraula"
        :class="{ 'paraula-activa': index === estatDelJoc.indexParaulaActiva }"
      >
        <div v-if="index === estatDelJoc.indexParaulaActiva">
          <span v-for="(caracter, index) in paraula.text.split('')"
            :key="index"
            :class=getClassLletra(index)
          > {{ caracter }}
        </span></div>
        <div v-else>{{ paraula.text }}</div>
      </div>
    </div>
    <input 
      type="text" 
      class="text-input"
      v-model="estatDelJoc.textEntrat"
      @input="validarProgres"
      placeholder="Comença a escriure..."
    />
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const estatDelJoc = ref({
  // Llista de paraules a escriure. Cada paraula és un objecte.
  paraules: [
    { id: 1, text: 'component', estat: 'pendent', error:0},
    { id: 2, text: 'reactivitat', estat: 'pendent', error:0},
    { id: 3, text: 'javascript', estat: 'pendent', error:0},
    { id: 4, text: 'framework', estat: 'pendent', error:0},
    { id: 5, text: 'template', estat: 'pendent', error:0}
  ],
  // L'índex de la paraula que l'usuari ha d'escriure ara mateix.
  indexParaulaActiva: 0,
  // El text que l'usuari està introduint a l'input.
  textEntrat: '',
  // Un array on guardarem els resultats de cada paraula.
  estadistiques: [],
});

// Compara l'última lletra introduïda per canviar entre classes en funció si són iguals (Correcte) o no (Incorrecte) 
function getClassLletra(indexLletra){
  const lletraEsperada = paraulaActiva.value.text[indexLletra];
  const lletraIntroduida = estatDelJoc.value.textEntrat[indexLletra];

  if (!lletraIntroduida) {
    return '';
  } else if (lletraIntroduida === lletraEsperada) {
    return 'correcta'
  } else {
    return 'incorrecta'
  }
}

// Afegeix també una propietat computada per accedir fàcilment a la paraula activa
const paraulaActiva = computed(() => {
  return estatDelJoc.value.paraules[estatDelJoc.value.indexParaulaActiva];
});

let tempsIniciParaula = 0;

function iniciarCronometreParaula() {
  tempsIniciParaula = Date.now();
}

// Funció principal que s'executa a cada pulsació
function validarProgres() {
  // Iniciem el cronòmetre només quan es comença a escriure la primera lletra
  if (estatDelJoc.value.textEntrat.length === 1 && tempsIniciParaula === 0) {
    iniciarCronometreParaula();
  }

  //Compara 
  if (!paraulaActiva.value.text.startsWith(estatDelJoc.value.textEntrat)){
    paraulaActiva.value.error++
  }

  // Comprovem si la paraula escrita és igual a la paraula activa
  if (estatDelJoc.value.textEntrat === paraulaActiva.value.text) {
    const tempsTrigat = Date.now() - tempsIniciParaula;
    
    // Desem les estadístiques
    estatDelJoc.value.estadistiques.push({
      paraula: paraulaActiva.value.text,
      temps: tempsTrigat,
      error: paraulaActiva.value.error
    });

    // Marquem la paraula com a completada
    paraulaActiva.value.estat = 'completada';

    // Passem a la següent paraula
    estatDelJoc.value.indexParaulaActiva++;
    
    // Netegem l'input i reiniciem el cronòmetre
    estatDelJoc.value.textEntrat = '';
    tempsIniciParaula = 0;

    // Si hi ha una següent paraula
    if (estatDelJoc.value.indexParaulaActiva < estatDelJoc.value.paraules.length) {
      // (Podem afegir lògica addicional aquí si volem)
    } else {
      // Joc acabat!
      console.log('Joc acabat!', estatDelJoc.value.estadistiques);
    }
  }
}
</script>

<style>
.paraula-activa {
  font-weight: bold;
  background-color: yellow;
}

.correcta {
  color: green;
}

.incorrecta{
  color: red;
}
</style>

