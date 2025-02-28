<script>
  import { createEventDispatcher } from "svelte";
  import BlankField from "./BlankField.svelte";
  
  export let blanks;
  export let validated;
  
  const dispatch = createEventDispatcher();
  
  function handleDrop(event, index) {
    dispatch("dropWord", { event, blankIndex: index });
  }
  
  function handleKeyDown(event, index) {
    dispatch("handleKeyDown", { event, blankIndex: index });
  }
  
  function handleDragStartFromBlank(event, index) {
    dispatch("dragStartFromBlank", { event, blankIndex: index });
  }
</script>

<div class="space-y-6 text-lg font-medium text-gray-700">
  {#each blanks as blank, i}
    <p class="flex items-center gap-2">
      {#if i === 0}Une {/if}
      {#if i === 1}Une {/if}
      {#if i === 2}Une {/if}
      
      <BlankField
        {blank}
        {validated}
        on:drop={(e) => handleDrop(e.detail, i)}
        on:handleKeyDown={(e) => handleKeyDown(e.detail, i)}
        on:dragStartFromBlank={(e) => handleDragStartFromBlank(e.detail, i)}
      />
      
      {#if i === 0} est utilisée pour stocker des données. {/if}
      {#if i === 1} permet d'exécuter des tâches spécifiques. {/if}
      {#if i === 2} est utilisée pour répéter des instructions. {/if}
      
      {#if validated}
        <span class="text-2xl ml-2">
          {#if blank.isCorrect}
            ✅
          {:else}
            ❌
          {/if}
        </span>
      {/if}
    </p>
  {/each}
</div>