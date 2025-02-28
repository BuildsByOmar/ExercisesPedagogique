<script>
    import { createEventDispatcher } from 'svelte';
    export let descriptions;
    export let selectedDescription;
    const dispatch = createEventDispatcher();
  
    function handleSelect(description) {
      dispatch('selectDescription', description);
    }
  </script>
  
  <div>
    <h2 class="text-2xl font-bold mb-4 text-green-700 text-center">DESCRIPTION</h2>
    <ul class="space-y-4">
      {#each descriptions as description}
      <li>
        <button
          type="button"
          class="w-full min-w-[150px] text-center p-4 border rounded-lg transition-all duration-300
            {selectedDescription && selectedDescription.id === description.id ? 'bg-green-300 border-green-500' : 'hover:bg-green-200'}
            {selectedDescription && selectedDescription.id !== description.id ? 'opacity-50 cursor-not-allowed' : 'cursor-pointer'}"
          on:click={() => !selectedDescription || selectedDescription.id === description.id ? handleSelect(description) : null}
          disabled={selectedDescription && selectedDescription.id !== description.id}>
          {description.label}
        </button>
      </li>              
      {/each}
    </ul>
  </div>
  