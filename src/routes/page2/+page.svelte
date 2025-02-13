<script>
  // Données initiales des items et descriptions
  const originalItems = [
    { id: 1, label: "Variable", correctDescription: 2 },
    { id: 2, label: "Fonction", correctDescription: 1 },
    { id: 3, label: "Boucle", correctDescription: 3 }
  ];

  const originalDescriptions = [
    { id: 1, label: "Bloc de code réutilisable" },
    { id: 2, label: "Conteneur de données" },
    { id: 3, label: "Répéter une action" }
  ];

  let items = [...originalItems];  // Copie des items
  let descriptions = [...originalDescriptions];  // Copie des descriptions
  let associations = [];  // Liste des associations
  let selectedItem = null, selectedDescription = null;  // Sélection en cours
  let validated = false, score = 0;  // Validation et score

  // Sélectionner un item
  function selectItem(item) {
    if (validated) return;
    selectedItem = selectedItem && selectedItem.id === item.id ? null : item;
  }

  // Sélectionner une description
  function selectDescription(description) {
    if (validated) return;
    selectedDescription = selectedDescription && selectedDescription.id === description.id ? null : description;
  }

  // Ajouter une association
  function addAssociation() {
    if (selectedItem && selectedDescription) {
      associations = [...associations, { item: selectedItem, description: selectedDescription }];
      items = items.filter(i => i.id !== selectedItem.id);
      descriptions = descriptions.filter(d => d.id !== selectedDescription.id);
      selectedItem = selectedDescription = null;
    }
  }

  // Dissocier une association
  function dissociate(assocToRemove) {
    if (validated) return;
    associations = associations.filter(assoc => assoc !== assocToRemove);
    items = [...items, assocToRemove.item];
    descriptions = [...descriptions, assocToRemove.description];
    items.sort((a, b) => a.id - b.id);
    descriptions.sort((a, b) => a.id - b.id);
  }

  // Valider les associations et calculer le score
  function validateAssociations() {
    if (associations.length !== originalItems.length) {
      alert("Veuillez associer tous les éléments avant de valider !");
      return;
    }
    score = associations.reduce((acc, assoc) => {
      const correctDescriptionId = assoc.item.correctDescription;
      assoc.isCorrect = assoc.description.id === correctDescriptionId;
      return acc + (assoc.isCorrect ? 1 : 0);
    }, 0);
    validated = true;
  }

  // Réinitialiser le jeu
  function resetGame() {
    items = [...originalItems];
    descriptions = [...originalDescriptions];
    associations = [];
    selectedItem = selectedDescription = null;
    score = 0;
    validated = false;
  }
</script>

<!-- Interface utilisateur -->
<div class="max-w-4xl mx-auto p-6 bg-gradient-to-br from-blue-50 to-indigo-50 shadow-2xl rounded-3xl">
  <h1 class="text-4xl font-extrabold mb-8 text-center text-indigo-700">Jeu d'Association</h1>
  
  <!-- Items et Descriptions -->
  <div class="flex flex-col md:flex-row md:space-x-6">
    <div class="md:w-1/2 mb-6 md:mb-0">
      <h2 class="text-2xl font-bold mb-4 text-blue-700 text-center">Items</h2>
      <ul class="space-y-4">
        {#each items as item}
          <li>
            <button
              type="button"
              class="w-full text-left p-4 border rounded-lg transition-all duration-300
                {selectedItem && selectedItem.id === item.id ? 'bg-blue-300 border-blue-500' : 'hover:bg-blue-200'}
                {selectedItem && selectedItem.id !== item.id ? 'opacity-50 cursor-not-allowed' : 'cursor-pointer'}"
              on:click={() => !selectedItem || selectedItem.id === item.id ? selectItem(item) : null}
              disabled={selectedItem && selectedItem.id !== item.id}>
              {item.label}
            </button>
          </li>
        {/each}
      </ul>
    </div>
   
    <div class="md:w-1/2">
      <h2 class="text-2xl font-bold mb-4 text-green-700 text-center">Descriptions</h2>
      <ul class="space-y-4">
        {#each descriptions as description}
          <li>
            <button
              type="button"
              class="w-full text-left p-4 border rounded-lg transition-all duration-300
                {selectedDescription && selectedDescription.id === description.id ? 'bg-green-300 border-green-500' : 'hover:bg-green-200'}
                {selectedDescription && selectedDescription.id !== description.id ? 'opacity-50 cursor-not-allowed' : 'cursor-pointer'}"
              on:click={() => !selectedDescription || selectedDescription.id === description.id ? selectDescription(description) : null}
              disabled={selectedDescription && selectedDescription.id !== description.id}>
              {description.label}
            </button>
          </li>
        {/each}
      </ul>     
    </div>
  </div>
  
  <!-- Bouton pour Associer -->
  <div class="mt-6 flex justify-center">
    <button 
      class="px-6 py-3 bg-indigo-500 text-white rounded-lg shadow-lg hover:bg-indigo-600 transition-all duration-300
      disabled:opacity-50 disabled:cursor-not-allowed"
      on:click={addAssociation}
      disabled={!selectedItem || !selectedDescription || validated}>
      Associer
    </button>
  </div>

  <!-- Affichage des Associations -->
  <div class="mt-8">
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
                on:click={() => dissociate(assoc)}>
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

  <!-- Boutons pour Valider et Réinitialiser -->
  <div class="mt-8 flex justify-center space-x-6">
    {#if !validated}
      <button 
        class="px-6 py-3 bg-green-500 text-white rounded-lg shadow-lg hover:bg-green-600 transition-all duration-300
        disabled:opacity-50 disabled:cursor-not-allowed"
        class:cursor-not-allowed={associations.length !== originalItems.length}
        on:click={validateAssociations}
        disabled={associations.length !== originalItems.length}>
        Valider
      </button>
    {/if}
    <button 
      class="px-6 py-3 bg-gray-500 text-white rounded-lg shadow-lg hover:bg-gray-600 transition-all duration-300"
      on:click={resetGame}>
      Réinitialiser
    </button>
  </div>

  <!-- Affichage du Score -->
  {#if validated}
    <div class="mt-8 text-center">
      <h2 class="text-3xl font-extrabold text-indigo-700">Score : {score} / {originalItems.length}</h2>
      {#if score === originalItems.length}
        <p class="text-green-600 mt-4 font-semibold text-lg">🎉 Bravo ! Toutes les associations sont correctes !</p>
      {:else}
        <p class="text-red-600 mt-4 font-semibold text-lg">Certaines associations sont incorrectes. Revoyez vos réponses !</p>
      {/if}
    </div>
  {/if}
</div>