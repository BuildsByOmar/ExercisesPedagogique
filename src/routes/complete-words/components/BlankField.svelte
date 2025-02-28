<script>
  import { createEventDispatcher } from "svelte";
  
  export let blank;
  export let validated;
  
  const dispatch = createEventDispatcher();
  
  function allowDrop(event) {
    event.preventDefault();
  }
  
  function onDrop(event) {
    dispatch("drop", event);
  }
  
  function onKeyDown(event) {
    dispatch("handleKeyDown", event);
  }
  
  function onDragStart(event) {
    dispatch("dragStartFromBlank", event);
  }
</script>

<span
  role="textbox"
  aria-label={"Zone de texte pour " + blank.correctAnswer}
  aria-live="polite"
  tabindex="0"
  on:dragover={!validated ? allowDrop : null}
  on:drop={!validated ? onDrop : null}
  on:keydown={!validated ? onKeyDown : null}
  class="inline-flex justify-center items-center min-w-[120px] px-2 py-1 border-2 text-lg transition-all duration-300 rounded-lg text-center
    {blank.isCorrect === true ? 'bg-green-200 border-green-600 text-green-900' : ''}
    {blank.isCorrect === false ? 'bg-red-200 border-red-600 text-red-900' : 'border-gray-500 bg-gray-100'}"
  draggable={!validated && blank.userInput ? true : false}
  on:dragstart={!validated && blank.userInput ? onDragStart : null}
>
  {#if blank.userInput}
    <span class="cursor-grab">{blank.userInput}</span>
  {:else}
    <span class="text-gray-500 italic">Glissez un mot ici...</span>
  {/if}
</span>