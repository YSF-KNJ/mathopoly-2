<script>
  import { onMount } from 'svelte';

  let originalWord = '';
  let result = '';
  let correct = false;
  let keys = [];
  let code = [];
  let userWord = '';

  onMount(() => {
    const data = JSON.parse(localStorage.getItem('mathopoly-data'));
    originalWord = data?.solution?.toLowerCase() || '';
    keys = data?.winnerKeys || [];

    if (originalWord && keys.length) {
      const letters = originalWord.toUpperCase().split('');
      code = letters.map(letter => {
        const match = keys.find(k => k.char.toUpperCase() === letter);
        return match?.num ?? '?';
      });
    }
  });

  function checkWord() {
    if (userWord.trim().toLowerCase() === originalWord) {
      result = '🎉 Félicitations ! Vous avez trouvé le mot secret !';
      correct = true;
    } else {
      result = '❌ Mot incorrect. Réessayez !';
      correct = false;
    }
  }
</script>

<div class="final-container">
  <h1>🔐 Phase Finale : Décryptez le mot</h1>

  {#if code.length}
    <p>Voici le code chiffré :</p>
    <p class="code-display">{code.join(' - ')}</p>
  {/if}

  <p>Entrez le mot que vous pensez correspondre au code :</p>
  <input type="text" bind:value={userWord} placeholder="Entrez le mot..." />
  <button on:click={checkWord}>Vérifier</button>

  {#if result}
    <p class:success={correct} class:error={!correct}>{result}</p>
  {/if}
</div>

<style>
  .final-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    text-align: center;
  }
  input {
    padding: 10px;
    font-size: 1.2rem;
    border-radius: 8px;
    border: 1px solid #ccc;
    margin-top: 1rem;
    width: 300px;
    text-align: center;
  }
  button {
    margin-top: 1rem;
    padding: 10px 20px;
    font-size: 1rem;
    background: #1d3557;
    color: white;
    border: none;
    border-radius: 8px;
    cursor: pointer;
  }
  button:hover {
    background: #457b9d;
  }
  .code-display {
    font-family: monospace;
    font-size: 1.3rem;
    margin: 0.5rem 0 1rem;
    color: #1d3557;
  }
  .success {
    color: green;
    font-weight: bold;
    margin-top: 1rem;
    font-size: 1.2rem;
  }
  .error {
    color: red;
    font-weight: bold;
    margin-top: 1rem;
    font-size: 1.1rem;
  }
</style>
