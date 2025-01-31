<script>
  // liste des items à associer
  let items = [
    { name: "Variable", color: "#FF6347", selected: false, associatedIndex: null },
    { name: "Fonction", color: "#4682B4", selected: false, associatedIndex: null }
  ];

  // liste des descriptions à associer
  let descriptions = [
    { text: "Une sorte de conteneur pour stocker des données.", selected: false, associatedIndex: null, color: null },
    { text: "Un bloc de code qui peut être appelé pour effectuer une tâche spécifique.", selected: false, associatedIndex: null, color: null }
  ];

  // état de validattion (true si toutes les associations sont faites)
  let isValid = false;

  // vérifier l'état de validité
  function checkValidState() {
    isValid = items.every(function(item) {
      return item.associatedIndex !== null;
    });
  }

  // sélectionner un item
  function selectItem(index) {
    items[index].selected = !items[index].selected;

    // si on désélectionne un item, dissocier sa description associée
    if (!items[index].selected) {
      var descIndex = items[index].associatedIndex;
      if (descIndex !== null) {
        descriptions[descIndex].selected = false;
        descriptions[descIndex].associatedIndex = null;
        descriptions[descIndex].color = null;
        items[index].associatedIndex = null;
      }
    }
    checkValidState();
  }

  // sélectionner une description
  function selectDescription(index) {
    var selectedItem = items.find(function(item) {
      return item.selected && item.associatedIndex === null;
    });
    if (selectedItem) {
      selectedItem.associatedIndex = index;
      descriptions[index].associatedIndex = items.indexOf(selectedItem);
      descriptions[index].selected = true;
      descriptions[index].color = selectedItem.color;
      selectedItem.selected = false; // =désélectionner l'item après association
    }
    checkValidState();
  }

  // réinitialiser les sélections
  function deselectItems() {
    items = items.map(function(item) {
      return { ...item, selected: false, associatedIndex: null };
    });
    descriptions = descriptions.map(function(desc) {
      return { ...desc, selected: false, associatedIndex: null, color: null };
    });
    isValid = false;
  }
</script>

<div class="flex justify-around p-5">
  <div class="flex flex-col gap-2">
    {#each items as { name, color, selected }, index}
      <button
        class="px-4 py-2 text-white font-bold rounded shadow transition transform hover:scale-105"
        style="background-color: {color}; border: {selected ? '3px solid black' : '2px solid #ddd'}"
        on:click={() => selectItem(index)}
      >
        {name}
      </button>
    {/each}
  </div>

  <div class="flex flex-col gap-2">
    {#each descriptions as { text, selected, associatedIndex, color }, index}
      <button
        class="px-4 py-2 font-bold rounded shadow transition transform hover:scale-105"
        style="background-color: {color ? color : 'white'}; color: {color ? 'white' : 'black'}; border: {color ? '3px solid black' : '2px solid #ddd'}"
        on:click={() => selectDescription(index)}
      >
        {text}
      </button>
    {/each}
  </div>
</div>

<div class="text-center mt-5">
  <button class="px-4 py-2 bg-gray-400 text-white font-bold rounded shadow transition transform hover:scale-105 disabled:opacity-50" on:click={() => deselectItems()} disabled={!isValid}>Réinitialiser</button>
  <button class="px-4 py-2 bg-green-500 text-white font-bold rounded shadow transition transform hover:scale-105 disabled:opacity-50" disabled={!isValid}>Valider</button>
</div>
