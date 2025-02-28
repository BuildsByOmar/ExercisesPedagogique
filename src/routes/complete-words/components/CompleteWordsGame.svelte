<script>
  import { goto } from "$app/navigation";
  import BlanksContainer from "./BlanksContainer.svelte";
  import WordsList from "./WordsList.svelte";
  import ScoreDisplay from "./ScoreDisplay.svelte";
  import ActionButtons from "./ActionButtons.svelte";

  // Données initiales
  const originalWords = ["variable", "fonction", "boucle"];
  let words = [...originalWords];

  let blanks = [
    { id: 0, correctAnswer: "variable", userInput: "", placeholder: "", isCorrect: null },
    { id: 1, correctAnswer: "fonction", userInput: "", placeholder: "", isCorrect: null },
    { id: 2, correctAnswer: "boucle", userInput: "", placeholder: "", isCorrect: null }
  ];

  // Variables pour la gestion du drag
  let draggedWord = null;
  let draggedFromBlank = null;
  let validated = false;

  function allowDrop(event) {
    event.preventDefault();
  }

  function dragStartHandler(event, word) {
    if (validated) return;
    draggedWord = word;
    draggedFromBlank = null;
    event.dataTransfer.setData("text/plain", word);
    event.dataTransfer.effectAllowed = "move";
  }

  function dragStartFromBlank(event, blankIndex) {
    if (validated) return;
    draggedWord = blanks[blankIndex].userInput;
    draggedFromBlank = blankIndex;
    event.dataTransfer.setData("text/plain", draggedWord);
    event.dataTransfer.effectAllowed = "move";
  }

  function dropHandler(event, blankIndex) {
    if (validated) return;
    event.preventDefault();
    const word = event.dataTransfer.getData("text/plain") || draggedWord;
    if (blanks.some((blank, index) => blank.userInput === word && index !== blankIndex)) {
      alert("Ce mot a déjà été utilisé !");
      return;
    }
    if (word) {
      let oldWord = blanks[blankIndex].userInput;
      blanks[blankIndex].userInput = word;
      blanks = [...blanks];
      if (draggedFromBlank === null) {
        words = words.filter(w => w !== word);
      }
      if (oldWord && oldWord !== word && !words.includes(oldWord)) {
        words = [...words, oldWord];
      }
    }
  }

  function handleKeyDown(event, blankIndex) {
    if (!validated && (event.key === "Enter" || event.key === " ") && draggedWord) {
      dropHandler(event, blankIndex);
    }
  }

  function dropToWordsList(event) {
    if (validated) return;
    event.preventDefault();
    const word = event.dataTransfer.getData("text/plain") || draggedWord;
    if (draggedFromBlank !== null) {
      blanks[draggedFromBlank].userInput = "";
      blanks[draggedFromBlank].isCorrect = null;
      blanks = [...blanks];
      draggedFromBlank = null;
    }
    if (word && !words.includes(word)) {
      words = [...words, word];
    }
  }

  function validateAnswers() {
    if (!blanks.every(blank => blank.userInput !== "")) return;
    blanks = blanks.map(blank => ({
      ...blank,
      isCorrect: blank.userInput === blank.correctAnswer
    }));
    validated = true;
  }

  function resetAnswers() {
    blanks = blanks.map(blank => ({
      ...blank,
      userInput: "",
      isCorrect: null
    }));
    words = [...originalWords];
    validated = false;
  }

  function goToNextExercise() {
    goto("/link-elements");
  }

  $: score = blanks.reduce((acc, blank) => acc + (blank.isCorrect ? 1 : 0), 0);
  $: scoreMessage = score === blanks.length
    ? "Félicitations ! Vous avez tout trouvé ! 🎉"
    : score >= Math.ceil(blanks.length / 2)
    ? "Pas mal ! Continuez comme ça ! 💪"
    : "Oups, réessaie ! 😅";
</script>

<div class="max-w-5xl mx-auto p-8 rounded-xl shadow-xl border border-gray-200 backdrop-blur-md bg-white/50">
  <h2 class="text-5xl font-extrabold text-center mb-10 bg-clip-text bg-gradient-to-r text-purple-600">
    Complétez le texte !
  </h2>

  <!-- Affichage des zones à remplir -->
  <BlanksContainer {blanks} {validated}
    on:dropWord={(e) => dropHandler(e.detail.event, e.detail.blankIndex)}
    on:handleKeyDown={(e) => handleKeyDown(e.detail.event, e.detail.blankIndex)}
    on:dragStartFromBlank={(e) => dragStartFromBlank(e.detail.event, e.detail.blankIndex)}
  />

  <!-- Affichage du score si validé -->
  {#if validated}
    <ScoreDisplay {score} blanksLength={blanks.length} scoreMessage={scoreMessage} />
  {/if}

  <!-- Affichage de la liste des mots si non validé -->
  {#if !validated}
    <WordsList {words} {allowDrop} {dropToWordsList} on:dragStartWord={(e) => dragStartHandler(e.detail.event, e.detail.word)} />
  {/if}

  <!-- Boutons d'action : maintenant on passe aussi "blanks" -->
  <ActionButtons 
    {validated}
    {blanks}
    on:validate={validateAnswers} 
    on:reset={resetAnswers} 
    on:nextExercise={goToNextExercise} />
</div>
