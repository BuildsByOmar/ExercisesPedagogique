<script>
  // Importation des composants nécessaires
  import { goto } from "$app/navigation";
  import ItemsList from './components/ItemsList.svelte';
  import DescriptionsList from './components/DescriptionsList.svelte';
  import AssociationsList from './components/AssociationsList.svelte';
  import ActionButtons from './components/ActionButtons.svelte';

  // Données initiales : liste des items (mots) et descriptions
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

  // Initialisation des variables d'état pour les éléments, descriptions et associations
  let items = [...originalItems];
  let descriptions = [...originalDescriptions];
  let associations = [];
  let selectedItem = null;
  let selectedDescription = null;
  let validated = false;
  let score = 0;

  
  // Fonction de gestion de la sélection d'un item
  function selectItem(item) {
    if (validated) return; // Si le jeu est validé, on ne fait rien
    if (selectedItem && selectedItem.id === item.id) {
      selectedItem = null; // Si l'item est déjà sélectionné, on le désélectionne
    } else {
      selectedItem = item; // Sinon, on sélectionne l'item
    }
  }

  // Fonction de gestion de la sélection d'une description
  function selectDescription(description) {
    if (validated) return; // Si le jeu est validé, on ne fait rien
    if (selectedDescription && selectedDescription.id === description.id) {
      selectedDescription = null; // Si la description est déjà sélectionnée, on la désélectionne
    } else {
      selectedDescription = description; // Sinon, on sélectionne la description
    }
  }

  // Fonction pour ajouter une association entre un item et une description
  function addAssociation() {
    if (selectedItem && selectedDescription) {
      // Ajout de l'association dans la liste des associations
      associations = [...associations, { item: selectedItem, description: selectedDescription }];
      // Retirer l'item et la description associée des listes disponibles
      items = items.filter(i => i.id !== selectedItem.id);
      descriptions = descriptions.filter(d => d.id !== selectedDescription.id);
      selectedItem = null; // Réinitialiser la sélection
      selectedDescription = null; // Réinitialiser la sélection
    }
  }

  // Fonction pour dissocier un item et une description
  function dissociate(assocToRemove) {
    if (validated) return; // Si le jeu est validé, on ne fait rien
    // Supprimer l'association de la liste
    associations = associations.filter(assoc => assoc !== assocToRemove);
    // Ajouter l'item et la description dissociée dans leurs listes respectives
    items = [...items, assocToRemove.item];
    descriptions = [...descriptions, assocToRemove.description];
    // Tri des éléments et descriptions pour garder l'ordre
    items.sort((a, b) => a.id - b.id);
    descriptions.sort((a, b) => a.id - b.id);
  }

  // Fonction pour valider les associations
  function validateAssociations() {
    if (associations.length !== originalItems.length) {
      alert("Veuillez associer tous les éléments avant de valider !");
      return; // Vérification que toutes les associations sont faites avant de valider
    }
    // Calcul du score en comparant les descriptions associées avec les descriptions correctes
    score = associations.reduce((acc, assoc) => {
      const correctDescriptionId = assoc.item.correctDescription;
      assoc.isCorrect = assoc.description.id === correctDescriptionId;
      return acc + (assoc.isCorrect ? 1 : 0);
    }, 0);
    validated = true; // Marquer le jeu comme validé
  }

  // Fonction pour réinitialiser le jeu
  function resetGame() {
    items = [...originalItems]; 
    descriptions = [...originalDescriptions]; 
    associations = []; 
    selectedItem = null; 
    selectedDescription = null; 
    score = 0; 
    validated = false; 
  }

  function goToNextExercise() {
    goto("/categories");
  }

</script>

<!-- Conteneur principal de l'application -->
<div class="max-w-5xl mx-auto p-8 rounded-xl shadow-xl border border-gray-200">

  <!-- Titre de l'application -->
  <h1 class="text-5xl font-extrabold text-center mb-10 text-purple-600">
    Associer les mots !
  </h1>

  <!-- Section Items et Descriptions -->
  <div class="flex flex-col md:flex-row justify-between items-start gap-8">
    <div class="w-full md:w-1/2">
      <ItemsList {items} {selectedItem} on:selectItem={e => selectItem(e.detail)} />
    </div>
    <div class="w-full md:w-1/2">
      <DescriptionsList {descriptions} {selectedDescription} on:selectDescription={e => selectDescription(e.detail)} />
    </div>
  </div>

  <!-- Boutons d'action pour ajouter une association, valider ou réinitialiser -->
  <div class="mt-8 flex flex-col sm:flex-row justify-center items-center gap-4">
    <ActionButtons 
      {selectedItem}
      {selectedDescription}
      {validated}
      {associations}
      {originalItems}
      on:addAssociation={addAssociation}
      on:validateAssociations={validateAssociations}
      on:resetGame={resetGame}
      on:nextExercise={goToNextExercise} />
  </div>

  <!-- Liste des associations faites -->
  <div class="mt-10">
    <AssociationsList {associations} {validated} on:dissociate={e => dissociate(e.detail)} />
  </div>

  <!-- Affichage du score après validation -->
  {#if validated}
  <div class="mt-8 flex justify-center">
    <div class="bg-gray-100 rounded-lg p-4 border border-gray-300 w-64 text-center
      {score === originalItems.length ? 'bg-green-400 text-white' : 
       score >= Math.ceil(originalItems.length / 2) ? 'bg-yellow-400 text-white' : 'bg-red-400 text-white'}">
      <h3 class="text-lg font-semibold">Votre score</h3>
      <div class="mt-2 text-3xl font-bold">
        {score} / {originalItems.length}
      </div>
      <p class="mt-3 text-lg text-white">
        {#if score === originalItems.length}
          🎉 Bravo ! Toutes les associations sont correctes !
        {:else if score >= Math.ceil(originalItems.length / 2)}
          Pas mal ! Continuez comme ça ! 💪
        {:else}
          Oups, réessaie ! 😅
        {/if}
      </p>
    </div>
  </div>
  {/if}

</div>
