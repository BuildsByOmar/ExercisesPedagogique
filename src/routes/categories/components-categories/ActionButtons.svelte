<script>
  import { createEventDispatcher } from "svelte";
  import { goto } from '$app/navigation';

  export let validated;
  export let categories;
  export let originalWords;
  const dispatch = createEventDispatcher();
 
  function validateAnswers() {
    dispatch('validate');
  }
 
  function resetAnswers() {
    dispatch('reset');
  }
 
  function goBack() {
    goto("/link-elements");  // Revenir à la page précédente
  }
 
  // Vérifier si tous les mots ont été placés dans des catégories
  $: totalWordsInCategories = categories ? categories.reduce((total, cat) => total + cat.words.length, 0) : 0;
  $: allWordsPlaced = totalWordsInCategories === originalWords.length;
</script>

<div class="mt-10 flex flex-col sm:flex-row justify-center items-center gap-6">
  {#if !validated}
    <button
      on:click={validateAnswers}
      aria-label="Valider les réponses"
      disabled={!allWordsPlaced}
      class="px-8 py-3 text-lg font-semibold rounded-lg shadow-md transition-all
        {allWordsPlaced ? 'bg-green-500 text-white hover:bg-green-600' : 'bg-gray-300 text-gray-500 cursor-not-allowed'}">
      ✅ Valider
    </button>
  {/if}
 
  <button
    on:click={resetAnswers}
    aria-label="Réinitialiser"
    disabled={!categories.some(cat => cat.words.length > 0)}
    class="px-8 py-3 text-lg font-semibold rounded-lg shadow-md transition-all
      {categories.some(cat => cat.words.length > 0) ? 'bg-yellow-500 text-white hover:bg-yellow-600' : 'bg-gray-300 text-gray-500 cursor-not-allowed'}">
    🔄 Réinitialiser
  </button>
   
  <button 
    class="px-8 py-3 text-lg bg-blue-500 text-white font-semibold rounded-lg shadow-md hover:bg-blue-600 transition-all"
    on:click={goBack}>
    ↩️ Retour
  </button>
</div>