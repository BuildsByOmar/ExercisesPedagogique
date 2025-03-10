<script>
  import { createEventDispatcher } from "svelte";

  export let categories;
  export let validated;

  const dispatch = createEventDispatcher();

  function handleDrop(event, index) {
    dispatch("dropWord", { event, categoryIndex: index });
  }

  function handleDragStart(event, categoryIndex, wordIndex) {
    dispatch("dragStartFromCategory", { event, categoryIndex, wordIndex });
  }

  // Fonction pour définir la classe CSS en fonction de la validation
  function getWordClass(word) {
    if (!validated) return "bg-blue-200 border-blue-500"; // Par défaut avant validation
    return word.isCorrect ? "bg-green-200 border-green-600 text-green-900" : "bg-red-200 border-red-600 text-red-900";
  }
</script>

<div class="grid grid-cols-1 sm:grid-cols-2 gap-8" role="list">
  {#each categories as category, index}
    <div class="p-6 border rounded-lg shadow-lg bg-white" role="group" aria-labelledby={"category-" + index}>
      <h3 id={"category-" + index} class="text-2xl font-semibold text-center mb-4">{category.name}</h3>

      <div class="min-h-[100px] p-4 border-dashed border-2 rounded-lg bg-gray-100"
        on:dragover|preventDefault
        on:drop={(e) => handleDrop(e, index)}
        role="list"
        aria-labelledby={"category-" + index}
        aria-live="polite">
       
        {#each category.words as word, wordIndex}
          <div
            draggable="true"
            on:dragstart={(e) => handleDragStart(e, index, wordIndex)}
            class={"px-4 py-2 rounded-lg m-1 inline-block border transition-all duration-300 cursor-pointer " + getWordClass(word)}
            role="listitem"
            aria-label={word.text || word}>
            {word.text || word}
          </div>       
        {/each}

        {#if category.words.length === 0}
          <p class="text-gray-400 text-center" aria-live="polite">Glissez un mot ici</p>
        {/if}
      </div>
    </div>
  {/each}
</div>
