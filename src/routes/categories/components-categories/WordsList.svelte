<script>
    import { createEventDispatcher } from "svelte";
  
    export let words = [];
    export let allowDrop;
    export let dropToWordsList;
  
    const dispatch = createEventDispatcher();
  
    function handleDragStart(event, word) {
      dispatch("dragStartWord", { event, word });
    }
  </script>
  
  <div class="mt-10" role="list" aria-label="Liste des mots à glisser" on:dragover={allowDrop} on:drop={dropToWordsList}>
    <h3 class="text-xl font-semibold text-indigo-700">Glissez et déposez les mots :</h3>
    <div class="flex flex-wrap gap-4 mt-4 justify-center border-2 border-dashed border-gray-400 p-4 rounded-lg">
      {#each words as word}
        <span
          role="listitem"
          aria-label={"Mot à glisser : " + word}
          aria-grabbed="false"
          draggable="true"
          tabindex="-1"
          on:dragstart={(e) => handleDragStart(e, word)}
          class="px-6 py-3 border-2 border-indigo-500 rounded-lg bg-indigo-100 text-indigo-700 font-semibold hover:bg-indigo-200 cursor-grab transition transform hover:-translate-y-1 shadow-md"
        >
          {word}
        </span>
      {/each}
    </div>
  </div>
  