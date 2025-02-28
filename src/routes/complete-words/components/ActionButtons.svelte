<script>
  import { createEventDispatcher } from "svelte";
  
  export let validated;
  export let blanks;
  const dispatch = createEventDispatcher();

  function validateAnswers() {
    dispatch('validate');
  }

  function resetAnswers() {
    dispatch('reset');
  }

  function goToNextExercise() {
    dispatch('nextExercise');
  }
</script>

<div class="mt-10 flex flex-col sm:flex-row justify-center items-center gap-6">
  {#if !validated}
    <button 
      on:click={validateAnswers} 
      aria-label="Valider les réponses" 
      disabled={!blanks.every(blank => blank.userInput !== "")}
      class="px-8 py-3 text-lg font-semibold rounded-lg shadow-md transition-all
        {blanks.every(blank => blank.userInput !== "") ? 'bg-green-500 text-white hover:bg-green-600' : 'bg-gray-300 text-gray-500 cursor-not-allowed'}">
      ✅ Valider
    </button>
  {/if}
  
  <button 
    on:click={resetAnswers} 
    aria-label="Réinitialiser"
    disabled={!blanks.some(blank => blank.userInput !== "")}
    class="px-8 py-3 text-lg font-semibold rounded-lg shadow-md transition-all
      {blanks.some(blank => blank.userInput !== "") ? 'bg-yellow-500 text-white hover:bg-yellow-600' : 'bg-gray-300 text-gray-500 cursor-not-allowed'}">
    🔄 Réinitialiser
  </button>

  <button 
    on:click={goToNextExercise} 
    aria-label="Exercice suivant"
    class="px-8 py-3 text-lg bg-blue-500 text-white font-semibold rounded-lg shadow-md hover:bg-blue-600 transition-all">
    ⏭️ Exercice Suivant
  </button>
</div>
