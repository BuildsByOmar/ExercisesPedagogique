<script>
  // liste des éléments à associer
  let items = [
    { name: "Variable", selected: false, associatedIndex: null },
    { name: "Fonction", selected: false, associatedIndex: null }
  ];

  // liste des descriptions à associer
  let descriptions = [
    { text: "Stocke des données", selected: false, associatedIndex: null },
    { text: "Exécute une tâche", selected: false, associatedIndex: null }
  ];

  // vaariable pour vérifier si toutes les associations sont faites
  let isValid = false;

  // vérifie si toutes les associations sont complètes
  function checkValidState() {
    isValid = items.every(item => item.associatedIndex !== null);
  }

  // sélectione un élément et gère la désélection
  function selectItem(index) {
    items[index].selected = !items[index].selected;
    if (!items[index].selected) {
      let descIndex = items[index].associatedIndex;
      if (descIndex !== null) {
        descriptions[descIndex].selected = false;
        descriptions[descIndex].associatedIndex = null;
        items[index].associatedIndex = null;
      }
    }
    checkValidState();
  }

  // sélectionne une description et l'associe à un élément sélectionné
  function selectDescription(index) {
    let selectedItem = items.find(item => item.selected && item.associatedIndex === null);
    if (selectedItem) {
      selectedItem.associatedIndex = index;
      descriptions[index].associatedIndex = items.indexOf(selectedItem);
      descriptions[index].selected = true;
      selectedItem.selected = false;
    }
    checkValidState();
  }

  // réinitialise toutes les sélections
  function deselectItems() {
    items.forEach(item => {
      item.selected = false;
      item.associatedIndex = null;
    });
    descriptions.forEach(desc => {
      desc.selected = false;
      desc.associatedIndex = null;
    });
    isValid = false;
  }
</script>

<div class="flex justify-around p-5">
  <div class="flex flex-col gap-2">
    {#each items as { name, selected }, index}
      <button class="px-4 py-2 bg-blue-500 text-white rounded" on:click={() => selectItem(index)}>
        {name}
      </button>
    {/each}
  </div>

  <div class="flex flex-col gap-2">
    {#each descriptions as { text, selected }, index}
      <button class="px-4 py-2 bg-gray-200 rounded" on:click={() => selectDescription(index)}>
        {text}
      </button>
    {/each}
  </div>
</div>

<div class="text-center mt-5">
  <button class="px-4 py-2 bg-gray-400 text-white rounded" on:click={() => deselectItems()} disabled={!isValid}>Réinitialiser</button>
  <button class="px-4 py-2 bg-green-500 text-white rounded" disabled={!isValid}>Valider</button>
</div>
