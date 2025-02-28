<script>
    import { createEventDispatcher } from 'svelte';
    export let associations;
    export let validated;
    const dispatch = createEventDispatcher();
  
    function handleDissociate(assoc) {
      dispatch('dissociate', assoc);
    }
  </script>
  
  <div>
    <h2 class="text-2xl font-bold mb-4 text-gray-800">Associations Réalisées</h2>
    {#if associations.length > 0}
      <ul class="space-y-4">
        {#each associations as assoc}
          <li class="p-4 border rounded-lg flex justify-between items-center shadow-lg
            {validated
              ? (assoc.isCorrect ? 'bg-green-200 border-green-500' : 'bg-red-200 border-red-500')
              : 'bg-gray-200 border-gray-400'}">
            <span class="font-semibold">{assoc.item.label} ➡ {assoc.description.label}</span>
            {#if !validated}
              <button 
                class="text-red-500 hover:text-red-700 ml-4 font-medium"
                on:click={() => handleDissociate(assoc)}>
                Dissocier
              </button>
            {/if}
            {#if validated}
              <span class="font-bold text-lg">
                {assoc.isCorrect ? '✅ Correct' : '❌ Incorrect'}
              </span>
            {/if}
          </li>
        {/each}
      </ul>
    {:else}
      <p class="text-gray-500 italic">Aucune association réalisée pour l'instant.</p>
    {/if}
  </div>
  