<script lang="ts">
  import { onMount } from 'svelte';

  let audio: HTMLAudioElement;
  let playing = false;

  /** Lecture automatique (si autorisé) */
  onMount(() => {
    audio = new Audio('/music/main-theme.mp3');
    audio.loop = true;
    audio.volume = 0.3;
    tryPlay();
  });

  function tryPlay() {
    audio.play().then(() => (playing = true)).catch(() => (playing = false));
  }

  function toggleMusic() {
    playing ? audio.pause() : tryPlay();
    playing = !playing;
  }
</script>

<svelte:head>
  <title>Mathopoly</title>
</svelte:head>

<main>
  <button class="music-btn" on:click={toggleMusic}>
    {playing ? '🔊 Pause' : '▶️ Musique'}
  </button>
  <slot />
</main>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@500&display=swap');
  :global(body) {
    margin: 0;
    background: linear-gradient(135deg, #f0f4f8, #dfe6ed);
    font-family: 'Quicksand', sans-serif;
  }
  main {
    padding: 1.5rem;
    max-width: 1200px;
    margin: auto;
    position: relative;
    min-height: 100vh;
    box-sizing: border-box;
  }
  .music-btn {
    position: fixed;
    top: 15px;
    right: 15px;
    background: #fff;
    border: 2px solid #0077b6;
    color: #0077b6;
    border-radius: 12px;
    padding: 8px 16px;
    font-size: 0.9rem;
    cursor: pointer;
    transition: 0.3s;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
    z-index: 999;
  }
  .music-btn:hover {
    background: #0077b6;
    color: #fff;
  }
</style>
