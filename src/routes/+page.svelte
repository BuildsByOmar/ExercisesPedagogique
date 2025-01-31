<script>
  import { goto } from "$app/navigation"; // Import pour naviguer entre les pages

  // Liste des mots à glisser
  let solutions = ["variable", "fonction", "boucle"];

  // Zones de texte avec réponse correcte, réponse utilisateur et un placeholder
  let blanks = [
    { correctAnswer: "variable", userInput: "", placeholder: "_____________" },
    { correctAnswer: "fonction", userInput: "", placeholder: "_____________" },
    { correctAnswer: "boucle", userInput: "", placeholder: "_____________" }
  ];

  // Permet de déposer un élément (empêche le comportement par défaut)
  function allowDrop(event) {
    event.preventDefault();
  }

  // Gère l'action de déposer un mot dans une zone
  function dropHandler(event, blank) {
    event.preventDefault(); // Empêche le comportement par défaut
    const word = event.dataTransfer.getData("text/plain"); // Récupère le mot glissé
    blank.userInput = word; // Affecte le mot à la zone correspondante
  }

  // Valide les réponses en comparant userInput à correctAnswer
  function validateAnswers() {
    blanks.forEach((blank) => {
      blank.isCorrect = blank.userInput === blank.correctAnswer; // Vrai si correct
    });
  }

  // Redirige vers la page 2
  function navigateToPage2() {
    goto("/page2");
  }
</script>

<style>
  /* Style pour les zones correctes */
  .correct {
    color: green;
    font-weight: bold;
  }

  /* Style pour les zones incorrectes */
  .incorrect {
    color: red;
    font-weight: bold;
  }

  /* Style des mots à glisser */
  .draggable {
    border: 1px solid #ccc;
    padding: 5px;
    margin: 5px;
    background-color: #f0f0f0;
    cursor: grab;
    display: inline-block;
  }

  /* Style des zones où déposer les mots */
  .droppable {
    border-bottom: 1px dotted black;
    padding: 5px;
    display: inline-block;
    min-width: 100px;
  }

  /* Style général pour la mise en page */
  .container {
    font-family: monospace;
    font-size: 16px;
  }

  /* Style pour le bouton */
  button {
    margin-top: 20px;
    padding: 10px 20px;
    background-color: #4CAF50;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  button:hover {
    background-color: #45a049;
  }
</style>

<div class="container">
  <p>Complétez les phrases suivantes avec les mots corrects :</p>

  <!-- Première phrase avec une zone de texte vide -->
  <p>
    Une 
    <span
      role="textbox" 
      aria-label="Zone de texte à compléter pour le mot 1"
      tabindex="0" 
      class="droppable"
      on:drop={(event) => dropHandler(event, blanks[0])} 
      on:dragover={allowDrop} 
      class:correct={blanks[0].isCorrect} 
      class:incorrect={blanks[0].isCorrect === false} 
    >
      {blanks[0].userInput || blanks[0].placeholder} <!-- Affiche l'entrée ou le placeholder -->
    </span>
    est une sorte de conteneur pour stocker des données variables.
  </p>

  <!-- Deuxième phrase -->
  <p>
    Une 
    <span
      role="textbox"
      aria-label="Zone de texte à compléter pour le mot 2"
      tabindex="0"
      class="droppable"
      on:drop={(event) => dropHandler(event, blanks[1])}
      on:dragover={allowDrop}
      class:correct={blanks[1].isCorrect}
      class:incorrect={blanks[1].isCorrect === false}
    >
      {blanks[1].userInput || blanks[1].placeholder}
    </span>
    est un bloc de code qui accomplit une tâche spécifique.
  </p>

  <!-- Troisième phrase -->
  <p>
    Une 
    <span
      role="textbox"
      aria-label="Zone de texte à compléter pour le mot 3"
      tabindex="0"
      class="droppable"
      on:drop={(event) => dropHandler(event, blanks[2])}
      on:dragover={allowDrop}
      class:correct={blanks[2].isCorrect}
      class:incorrect={blanks[2].isCorrect === false}
    >
      {blanks[2].userInput || blanks[2].placeholder}
    </span>
    est utilisée pour répéter une action plusieurs fois.
  </p>

  <!-- Liste des mots à glisser -->
  <p>Glissez-déposez les mots suivants :</p>
  <div>
    {#each solutions as word}
      <span
        role="button" 
        aria-grabbed="false"
        draggable="true"
        tabindex="0"
        class="draggable"
        on:dragstart={(event) => event.dataTransfer.setData("text/plain", word)}
      >
        {word}
      </span>
    {/each}
  </div>

  <!-- Bouton pour valider les réponses -->
  <button on:click={validateAnswers} tabindex="0">Valider</button>

  <!-- Bouton pour naviguer vers la page 2 -->
  <button on:click={navigateToPage2} tabindex="0">Aller à l'exercice suivant</button>
</div>
