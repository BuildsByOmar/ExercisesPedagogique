<script>
  import { goto } from "$app/navigation";
  import CategoriesContainer from "./components-categories/CategoriesContainer.svelte";
  import WordsList from "./components-categories/WordsList.svelte";
  import ActionButtons from "./components-categories/ActionButtons.svelte";
  import ScoreDisplay from "../complete-words/components/ScoreDisplay.svelte"; // Import du composant ScoreDisplay

  // Définition des catégories
  let categories = [
    { id: 0, name: "Langages de programmation", words: [], correctWords: ["JavaScript", "Python", "C++"] },
    { id: 1, name: "Concepts informatiques", words: [], correctWords: ["Algorithme", "Boucle", "Variable"] }
  ];

  // Liste initiale des mots
  const originalWords = ["JavaScript", "Python", "C++", "Algorithme", "Boucle", "Variable"];
  let words = [...originalWords];

  let draggedWord = null;
  let draggedFromCategory = null;
  let validated = false;

  let score = 0;
  let totalWords = originalWords.length;
  let scoreMessage = "";

  function allowDrop(event) {
    event.preventDefault();
  }

  function dragStartHandler(event, word) {
    if (validated) return;
    draggedWord = word;
    draggedFromCategory = null;
    event.dataTransfer.setData("text/plain", word);
  }

  function dragStartFromCategory(event, categoryIndex, wordIndex) {
    if (validated) return;
    draggedWord = categories[categoryIndex].words[wordIndex];
    draggedFromCategory = { categoryIndex, wordIndex };
    event.dataTransfer.setData("text/plain", draggedWord);
  }

  function dropHandler(event, categoryIndex) {
    if (validated) return;
    event.preventDefault();
    const word = event.dataTransfer.getData("text/plain") || draggedWord;

    if (!word) return;
    if (categories.some(cat => cat.words.includes(word) && cat !== categories[categoryIndex])) {
      alert("Ce mot est déjà dans une catégorie !");
      return;
    }

    let oldWord = draggedFromCategory ? categories[draggedFromCategory.categoryIndex].words.splice(draggedFromCategory.wordIndex, 1)[0] : null;

    categories[categoryIndex].words.push(word);
    categories = [...categories];

    if (draggedFromCategory === null) {
      words = words.filter(w => w !== word);
    }

    if (oldWord && oldWord !== word && !words.includes(oldWord)) {
      words = [...words, oldWord];
    }
  }

  function dropToWordsList(event) {
    if (validated) return;
    event.preventDefault();
    const word = event.dataTransfer.getData("text/plain") || draggedWord;

    if (draggedFromCategory !== null) {
      categories[draggedFromCategory.categoryIndex].words.splice(draggedFromCategory.wordIndex, 1);
      categories = [...categories];
    }

    if (word && !words.includes(word)) {
      words = [...words, word];
    }
  }

  function calculateScore() {
    let correctCount = categories.reduce((acc, category) =>
      acc + category.words.filter(word => word.isCorrect).length, 0
    );

    return {
      score: correctCount,
      totalWords: originalWords.length,
      message: correctCount === originalWords.length
        ? "Bravo ! Tous les mots sont bien placés ! 🎉"
        : correctCount >= Math.ceil(originalWords.length / 2)
        ? "Pas mal, encore un effort ! 💪"
        : "Tu peux faire mieux ! Réessaie ! 😅"
    };
  }

  function validateAnswers() {
    categories = categories.map(category => ({
      ...category,
      words: category.words.map(word => ({
        text: word,
        isCorrect: category.correctWords.includes(word.text) // Vérifie si le mot est correct
      }))
    }));

    validated = true;

    // Met à jour le score après validation
    const result = calculateScore();
    score = result.score;
    scoreMessage = result.message;
  }


  function resetAnswers() {
    categories = categories.map(category => ({
      ...category,
      words: []
    }));
    words = [...originalWords];
    validated = false;
    score = 0;
    scoreMessage = "";
  }

  function goToNextExercise() {
    goto("/");
  }
</script>

<div class="max-w-5xl mx-auto p-8 rounded-xl shadow-xl border border-gray-200 bg-white/50">
  <h2 class="text-5xl font-extrabold text-center mb-10 text-purple-600">
    Associez les mots aux bonnes catégories !
  </h2>

  <CategoriesContainer {categories} {validated} 
    on:dropWord={(e) => dropHandler(e.detail.event, e.detail.categoryIndex)}
    on:dragStartFromCategory={(e) => dragStartFromCategory(e.detail.event, e.detail.categoryIndex, e.detail.wordIndex)}
  />

  {#if validated}
    <ScoreDisplay {score} blanksLength={totalWords} scoreMessage={scoreMessage} />
  {/if}

  {#if !validated}
    <WordsList {words} {allowDrop} {dropToWordsList} 
      on:dragStartWord={(e) => dragStartHandler(e.detail.event, e.detail.word)}
    />
  {/if}

  <ActionButtons {validated} {categories} {originalWords} on:validate={validateAnswers} on:reset={resetAnswers} on:nextExercise={goToNextExercise} />
</div>
