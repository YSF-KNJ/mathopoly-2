<script>
  import { onMount } from 'svelte';

  let data;
  let questions = [];
  let currentPlayer = 0;
  let players = ['X', 'Y', 'Z'];
  let playerColors = ['#e63946', '#457b9d', '#2a9d8f'];
  let cellColors = Array(49).fill(null);
  let showPopup = false;
  let popupContent = '';
  let dice1 = '🎲';
  let dice2 = '🎲';
  let backgroundAudio;
  let challengeAudio;

  onMount(() => {
    data = JSON.parse(localStorage.getItem('mathopoly-data'));
    questions = data.questions;

    backgroundAudio = new Audio('/music/background.mp3');
    backgroundAudio.loop = true;
    backgroundAudio.volume = 0.3;
    backgroundAudio.play();

    challengeAudio = new Audio('/music/challenge.mp3');
    challengeAudio.volume = 0.7;
  });

  function isEdge(i) {
    const row = Math.floor(i / 7);
    const col = i % 7;
    return row === 0 || row === 6 || col === 0 || col === 6;
  }

  function getEdgeIndices() {
    const indices = [];
    for (let i = 0; i < 49; i++) {
      if (isEdge(i)) indices.push(i);
    }
    return indices;
  }

  function handleCellClick(i) {
    if (!isEdge(i)) return;
    cellColors[i] = playerColors[currentPlayer];
    currentPlayer = (currentPlayer + 1) % players.length;

    const edgeIndices = getEdgeIndices();
    const allColored = edgeIndices.every(index => cellColors[index]);
    if (allColored) {
      window.location.href = '/final';
    }
  }

  function showQuestion(i) {
    if (!isEdge(i)) return;
    const edgeIndices = getEdgeIndices();
    const questionIndex = edgeIndices.indexOf(i);
    popupContent = questions[questionIndex] || 'Pas de question pour cette case.';
    showPopup = true;
    challengeAudio.currentTime = 0;
    challengeAudio.play();
  }

  function rollDice() {
    const d1 = Math.floor(Math.random() * 6) + 1;
    const d2 = Math.floor(Math.random() * 6) + 1;
    dice1 = d1;
    dice2 = d2;
  }
</script>

<div class="game-container">
  <h1>🎲 Plateau de Mathopoly</h1>
  <p>Tour du joueur : <strong>{players[currentPlayer]}</strong></p>

  <div class="board">
    {#each Array(49) as _, i}
      <div class="cell-wrapper">
        <div
          class="cell"
          on:click={() => handleCellClick(i)}
          style="background: {cellColors[i] ? cellColors[i] : (isEdge(i) ? 'white' : '#f1f1f1')};">
          {#if isEdge(i) && !cellColors[i]}
            <button class="question-icon" on:click|stopPropagation={() => showQuestion(i)}>?</button>
          {/if}
        </div>
      </div>
    {/each}
  </div>

  <div class="dice-area">
    <div class="dice">{dice1}</div>
    <div class="dice">{dice2}</div>
    <button on:click={rollDice}>🎲 Lancer les dés</button>
  </div>
</div>

{#if showPopup}
  <div class="popup">
    <div class="popup-inner">
      <p>{popupContent}</p>
      <button on:click={() => {
        showPopup = false;
        challengeAudio.pause();
        challengeAudio.currentTime = 0;
      }}>Fermer</button>
    </div>
  </div>
{/if}

<style>
  .game-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    text-align: center;
  }
  .board {
    display: grid;
    grid-template-columns: repeat(7, 60px);
    grid-template-rows: repeat(7, 60px);
    gap: 2px;
    margin-top: 1.5rem;
    position: relative;
  }
  .cell-wrapper {
    position: relative;
  }
  .cell {
    width: 60px;
    height: 60px;
    border: 1px solid #333;
    cursor: pointer;
    box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.1);
    position: relative;
  }
  .question-icon {
    position: absolute;
    top: 2px;
    left: 50%;
    transform: translateX(-50%);
    width: 14px;
    height: 14px;
    font-size: 10px;
    line-height: 14px;
    text-align: center;
    padding: 0;
    background: #e63946;
    color: white;
    border: none;
    border-radius: 3px;
    cursor: pointer;
  }
  .question-icon:hover {
    background: #d62828;
  }
  .dice-area {
    display: flex;
    align-items: center;
    gap: 15px;
    margin-top: 2rem;
  }
  .dice {
    width: 50px;
    height: 50px;
    font-size: 1.5rem;
    border: 2px solid #222;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 10px;
    background-color: white;
  }
  button {
    padding: 10px 20px;
    background: #1d3557;
    color: white;
    border: none;
    border-radius: 10px;
    font-size: 1rem;
    cursor: pointer;
    transition: 0.3s;
  }
  button:hover {
    background: #457b9d;
  }
  .popup {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.6);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 999;
  }
  .popup-inner {
    background: white;
    padding: 2rem;
    border-radius: 12px;
    max-width: 400px;
    text-align: center;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
  }
  .popup-inner p {
    font-size: 1.1rem;
    margin-bottom: 1rem;
  }
  .popup-inner button {
    padding: 0.5rem 1rem;
    background: #1d3557;
    color: white;
    border: none;
    border-radius: 8px;
    cursor: pointer;
  }
  .popup-inner button:hover {
    background: #457b9d;
  }
</style>
