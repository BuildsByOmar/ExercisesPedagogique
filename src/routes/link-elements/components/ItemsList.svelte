<script>
    import { createEventDispatcher } from 'svelte';
    export let items;
    export let selectedItem;
    const dispatch = createEventDispatcher();
  
    function handleSelect(item) {
      dispatch('selectItem', item);
    }
  </script>
  
  <div>
    <h2 class="text-2xl font-bold mb-4 text-blue-700 text-center">ITEMS</h2>
    <ul class="space-y-4">
      {#each items as item}
      <li>
        <button
          type="button"
          class="w-full min-w-[150px] text-center p-4 border rounded-lg transition-all duration-300
            {selectedItem && selectedItem.id === item.id ? 'bg-blue-300 border-blue-500' : 'hover:bg-blue-200'}
            {selectedItem && selectedItem.id !== item.id ? 'opacity-50 cursor-not-allowed' : 'cursor-pointer'}"
            on:click={() => !selectedItem || selectedItem.id === item.id ? handleSelect(item) : null}
            disabled={selectedItem && selectedItem.id !== item.id}>
          {item.label}
        </button>
      </li>                 
      {/each}
    </ul>
  </div>
  