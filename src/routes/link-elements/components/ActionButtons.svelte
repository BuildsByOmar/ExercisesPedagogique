<script>
  import { createEventDispatcher } from 'svelte';
  import { goto } from '$app/navigation';
  
  export let selectedItem;
  export let selectedDescription;
  export let validated;
  export let associations;
  export let originalItems;
  const dispatch = createEventDispatcher();

  function addAssociation() {
    dispatch('addAssociation');
  }

  function validateAssociations() {
    dispatch('validateAssociations');
  }

  function resetGame() {
    dispatch('resetGame');
  }

  function goBack() {
    goto("/");  // Revenir à la page précédente
  }

  function goToNextExercise() {
    dispatch('nextExercise');
  }
</script>

<div class="mt-6 flex justify-center">
  <button 
    class="px-8 py-3 text-lg font-semibold rounded-lg shadow-md transition-all
      {selectedItem && selectedDescription && !validated ? 'bg-indigo-500 text-white hover:bg-indigo-600' : 'bg-gray-300 text-gray-500 cursor-not-allowed'}"
    on:click={addAssociation}
    disabled={!selectedItem || !selectedDescription || validated}>
    🔗 Associer
  </button>
</div>

<!-- Boutons Valider, Réinitialiser, et Retour -->
<div class="mt-8 flex flex-col sm:flex-row justify-center items-center gap-6">
  {#if !validated}
    <button 
      class="px-8 py-3 text-lg font-semibold rounded-lg shadow-md transition-all
        {associations.length === originalItems.length ? 'bg-green-500 text-white hover:bg-green-600' : 'bg-gray-300 text-gray-500 cursor-not-allowed'}"
      on:click={validateAssociations}
      disabled={associations.length !== originalItems.length}>
      ✅ Valider
    </button>
  {/if}

  <button 
    class="px-8 py-3 text-lg font-semibold rounded-lg shadow-md transition-all
      {associations.length > 0 ? 'bg-yellow-500 text-white hover:bg-yellow-600' : 'bg-gray-300 text-gray-500 cursor-not-allowed'}"
    on:click={resetGame}
    disabled={associations.length === 0}>
    🔄 Réinitialiser
  </button>

  <button 
    class="px-8 py-3 text-lg bg-blue-500 text-white font-semibold rounded-lg shadow-md hover:bg-blue-600 transition-all"
    on:click={goBack}>
    ↩️ Retour
  </button>

  <button
    on:click={goToNextExercise}
    aria-label="Exercice suivant"
    class="px-8 py-3 text-lg bg-blue-500 text-white font-semibold rounded-lg shadow-md hover:bg-blue-600 transition-all">
    ⏭️ Exercice Suivant
  </button>

</div>
