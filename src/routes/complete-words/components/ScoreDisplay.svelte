<script>
  export let score;
  export let blanksLength;
  export let scoreMessage;
</script>

<div class="mt-8 flex justify-center">
  <div class="rounded-lg p-4 border border-gray-300 w-64 text-center
    {score === blanksLength ? 'bg-green-400 text-white' : 
     score >= Math.ceil(blanksLength / 2) ? 'bg-yellow-400 text-white' : 'bg-red-400 text-white'}">
    <h3 class="text-lg font-semibold">Votre score</h3>
    <div class="mt-2 text-3xl font-bold">
      {score} / {blanksLength}
    </div>
    <p class="mt-3 text-lg text-white">
      {scoreMessage}
    </p>
  </div>
</div>
