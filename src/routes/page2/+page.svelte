<script>
  // Liste des éléments à associer
  let items = [
    { name: "Variable", color: "#FF6347", selected: false, associatedIndex: null },
    { name: "Fonction", color: "#4682B4", selected: false, associatedIndex: null }
  ];

  // Liste des descriptions à associer
  let descriptions = [
    { text: "Une sorte de conteneur pour stocker des données.", selected: false, associatedIndex: null, color: null },
    { text: "Un bloc de code qui peut être appelé pour effectuer une tâche spécifique.", selected: false, associatedIndex: null, color: null }
  ];

  let isValid = false;
  let isSelectionActive = false;
  let selectionType = null;

  function checkValidState() {
    isValid = items.every(item => item.associatedIndex !== null);
  }

  function selectItem(index) {
    if (items[index].associatedIndex !== null) {
      const descIndex = items[index].associatedIndex;
      descriptions[descIndex].associatedIndex = null;
      descriptions[descIndex].color = null;
      descriptions[descIndex].selected = false;
      items[index].associatedIndex = null;
      items[index].selected = false;
      isSelectionActive = false;
      selectionType = null;
      checkValidState();
      return;
    }

    const selectedDescIndex = descriptions.findIndex(desc => desc.selected);
    if (selectedDescIndex !== -1) {
      createAssociation(index, selectedDescIndex);
      return;
    }

    const newSelectedState = !items[index].selected;
    isSelectionActive = newSelectedState;
    selectionType = newSelectedState ? 'item' : null;

    items = items.map((item, i) => ({
      ...item,
      selected: i === index ? newSelectedState : false
    }));
  }

  function selectDescription(index) {
    if (descriptions[index].associatedIndex !== null) return;

    const selectedItemIndex = items.findIndex(item => item.selected);
    if (selectedItemIndex !== -1) {
      createAssociation(selectedItemIndex, index);
      return;
    }

    const newSelectedState = !descriptions[index].selected;
    isSelectionActive = newSelectedState;
    selectionType = newSelectedState ? 'description' : null;

    descriptions = descriptions.map((desc, i) => ({
      ...desc,
      selected: i === index ? newSelectedState : false
    }));
  }

  function createAssociation(itemIndex, descriptionIndex) {
    descriptions[descriptionIndex].associatedIndex = itemIndex;
    descriptions[descriptionIndex].color = items[itemIndex].color;
    descriptions[descriptionIndex].selected = false;
    items[itemIndex].associatedIndex = descriptionIndex;
    items[itemIndex].selected = false;
    isSelectionActive = false;
    selectionType = null;
    checkValidState();
  }

  function deselectItems() {
    items = items.map(item => ({
      ...item,
      selected: false,
      associatedIndex: null
    }));
    
    descriptions = descriptions.map(desc => ({
      ...desc,
      selected: false,
      associatedIndex: null,
      color: null
    }));
    
    isValid = false;
    isSelectionActive = false;
    selectionType = null;
  }
</script>

<div class="flex justify-around p-5">
  <div class="flex flex-col gap-2">
    {#each items as { name, color, selected, associatedIndex }, index}
      {@const isDisabled = isSelectionActive && selectionType === 'item' && !selected && associatedIndex === null}
      <button
        class="px-4 py-2 text-white font-bold rounded shadow transition transform hover:scale-105 disabled:opacity-50 {isDisabled ? 'cursor-not-allowed' : 'cursor-pointer'}"
        style="background-color: {color}; border: {selected ? '3px solid black' : '2px solid #ddd'}"
        on:click={() => selectItem(index)}
        disabled={isDisabled}
      >
        {name}
      </button>
    {/each}
  </div>

  <div class="flex flex-col gap-2">
    {#each descriptions as { text, selected, associatedIndex, color }, index}
      {@const isDisabled = isSelectionActive && selectionType === 'description' && !selected && associatedIndex === null}
      <button
        class="px-4 py-2 font-bold rounded shadow transition transform hover:scale-105 disabled:opacity-50 {isDisabled ? 'cursor-not-allowed' : 'cursor-pointer'}"
        style="background-color: {color ? color : 'white'}; color: {color ? 'white' : 'black'}; border: {selected ? '3px solid black' : '2px solid #ddd'}"
        on:click={() => selectDescription(index)}
        disabled={isDisabled}
      >
        {text}
      </button>
    {/each}
  </div>
</div>

<div class="text-center mt-5">
  <button 
    class="px-4 py-2 bg-gray-400 text-white font-bold rounded shadow transition transform hover:scale-105 disabled:opacity-50" 
    on:click={deselectItems}
  >
    Réinitialiser
  </button>
  <button 
    class="px-4 py-2 bg-green-500 text-white font-bold rounded shadow transition transform hover:scale-105 disabled:opacity-50 cursor-not-allowed" 
    disabled={!isValid}
  >
    Valider
  </button>
</div>