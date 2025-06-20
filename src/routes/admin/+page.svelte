<script>
  function shuffledAlphabet() {
    return 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'.split('').sort(() => 0.5 - Math.random());
  }

  const letters = shuffledAlphabet();

  let questions = Array(24).fill('');
  let solution = '';

  let keysX = Array(9).fill(0).map((_, i) => ({
    num: Math.floor(Math.random() * 90) + 10,
    char: letters[i]
  }));

  let keysY = Array(9).fill(0).map((_, i) => ({
    num: Math.floor(Math.random() * 90) + 10,
    char: letters[i + 9]
  }));

  let keysZ = Array(9).fill(0).map((_, i) => ({
    num: Math.floor(Math.random() * 90) + 10,
    char: letters[i + 18]
  }));

  let generatedCode = [];
  let message = '';
  let error = false;

  function saveGameData() {
    if (!solution.trim()) {
      message = '❌ Veuillez saisir un mot secret.';
      error = true;
      return;
    }

    const winnerKeys = [...keysX, ...keysY, ...keysZ];
    const upperSolution = solution.toUpperCase();
    const missing = upperSolution.split('').filter(c => !winnerKeys.find(k => k.char.toUpperCase() === c));

    if (missing.length > 0) {
      message = `❌ Les lettres suivantes sont absentes des clés : ${[...new Set(missing)].join(', ')}`;
      error = true;
      return;
    }

    generatedCode = upperSolution.split('').map(letter => {
      const match = winnerKeys.find(k => k.char.toUpperCase() === letter);
      return match.num;
    });

    const data = {
      questions,
      solution: solution.toLowerCase(),
      keys: { X: keysX, Y: keysY, Z: keysZ },
      winnerKeys,
      finalCode: generatedCode
    };
    localStorage.setItem('mathopoly-data', JSON.stringify(data));
    message = '✅ Données enregistrées avec succès !';
    error = false;
  }
</script>

<h1>🧑‍🏫 Paramétrage du jeu – Mathopoly</h1>

<h3>1. Questions (24 cases)</h3>
{#each questions as q, i}
  <div>
    <label>Question {i + 1} :</label>
    <input bind:value={questions[i]} placeholder="Écrivez la question..." />
  </div>
{/each}

<h3>2. Mot secret final</h3>
<input bind:value={solution} placeholder="Ex: maths" />

<h3>3. Clés du joueur X</h3>
{#each keysX as key, i}
  <input bind:value={keysX[i].num} placeholder="Numéro" size="5" /> -
  <input bind:value={keysX[i].char} placeholder="Lettre" size="5" /><br>
{/each}

<h3>4. Clés du joueur Y</h3>
{#each keysY as key, i}
  <input bind:value={keysY[i].num} placeholder="Numéro" size="5" /> -
  <input bind:value={keysY[i].char} placeholder="Lettre" size="5" /><br>
{/each}

<h3>5. Clés du joueur Z</h3>
{#each keysZ as key, i}
  <input bind:value={keysZ[i].num} placeholder="Numéro" size="5" /> -
  <input bind:value={keysZ[i].char} placeholder="Lettre" size="5" /><br>
{/each}

<h3>6. Sauvegarde</h3>
<button on:click={saveGameData}>💾 Enregistrer</button>
<a href="/game"><button>🎮 Lancer le jeu</button></a>

{#if message}
  <p class:error={error} class:success={!error}>{message}</p>
{/if}

{#if generatedCode.length > 0}
  <h3>🔐 Code généré :</h3>
  <p class="code-display">{generatedCode.join(' - ')}</p>
  <p class="success">✨ Donne ce code au joueur gagnant pour décrypter le mot secret !</p>
{/if}

<style>
  input {
    margin: 6px;
    padding: 6px;
    border-radius: 6px;
    border: 1px solid #aaa;
    font-size: 0.95rem;
    background: #fdfdfd;
  }
  label {
    font-weight: bold;
  }
  h3 {
    margin-top: 2rem;
    color: #1d3557;
  }
  button {
    margin-top: 1rem;
    margin-right: 1rem;
    padding: 8px 16px;
    background: #1d3557;
    color: white;
    border: none;
    border-radius: 8px;
    font-size: 0.95rem;
    cursor: pointer;
  }
  button:hover {
    background: #457b9d;
  }
  .code-display {
    font-family: monospace;
    font-size: 1.2rem;
    color: #1d3557;
    margin-top: 0.5rem;
  }
  .error {
    color: red;
    font-weight: bold;
    margin-top: 1rem;
  }
  .success {
    color: green;
    font-weight: bold;
    margin-top: 1rem;
  }
</style>