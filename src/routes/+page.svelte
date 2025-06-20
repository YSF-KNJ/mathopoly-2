<script lang="ts">
    import { goto } from '$app/navigation';
  
    function startGame() {
      goto('/admin');   
    }
  </script>
  
  <section class="hero">
    <img src="board.png" alt="Plateau Mathopoly" class="hero-img" />
    <h1>Bienvenue sur <span class="brand">Mathopoly</span></h1>
    <p class="subtitle">
      Un défi ludique pour&nbsp;réviser les <strong>mathématiques</strong> en s'amusant.
    </p>
  
    <button class="start-btn" on:click={startGame}>🚀 Commencer</button>
  </section>
  
  <!-- Guide -->
  <section class="guide">
    <h2>📖 Guide &amp; Instructions</h2>
    <ul>
      <li>📝 L'enseignant configure 24 questions ainsi que les clés depuis la page «&nbsp;Paramétrage&nbsp;».</li>
      <li>🎲 Trois joueurs lancent les dés et colorient les cases du plateau.</li>
      <li>❓ Cliquez sur <strong>?</strong> pour afficher la question d'une case et y répondre.</li>
      <li>🔑 Cumulez des clés, déchiffrez le code final et trouvez le mot secret&nbsp;!</li>
    </ul>
    <p class="author">Réalisé par:</p>
    <ul class="group">
        <li>• Kanjaa Youssef</li>
        <li>• Oulad Moussa Sara</li>  
        <li>• Hamodan Samya</li>
        <li>• Errboun Mariam</li>
    </ul>
    <!-- Encadrer par : Prof. Hamdane Karima-->
    <p class="author">Encadré par : Prof. Hamdane Karima</p>



  </section>
  
  <style>
    /* Section Hero */
    .hero {
      display: flex;
      flex-direction: column;
      align-items: center;
      text-align: center;
      margin-top: 4rem;
    }
    .hero-img {
      width: 180px;
      margin-bottom: 1rem;
    }
    h1 {
      font-size: 2.7rem;
      margin: 0;
      color: #1d3557;
    }
    .brand {
      color: #e63946;
    }
    .subtitle {
      font-size: 1.1rem;
      color: #457b9d;
      margin: 1rem 0 1.8rem;
    }
    .start-btn {
      background: #2a9d8f;
      color: #fff;
      border: none;
      border-radius: 12px;
      padding: 12px 28px;
      font-size: 1.1rem;
      cursor: pointer;
      transition: background 0.25s;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    }
    .start-btn:hover {
      background: #21867a;
    }
  
    /* Section Guide */
    .guide {
      background: #fff;
      border-radius: 16px;
      padding: 2rem;
      margin: 3.5rem auto 2rem;
      box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05);
      max-width: 800px;
    }
    .guide h2 {
      text-align: center;
      color: #1d3557;
      margin-bottom: 1rem;
    }
    .guide ul {
      list-style: none;
      padding: 0;
      font-size: 1rem;
      line-height: 1.6;
      color: #333;
    }
    .guide li {
      margin: 0.6rem 0;
    }
    .author {
      text-align: center;
      margin-top: 1.5rem;
      font-style: italic;
      color: #555;
      font-weight: bold;

    }
    .group {
      list-style-type: disc;
      list-style-position: inside;
      text-align: left;
      margin-top: 1.5rem;
      color: #555;
    }
  </style>
  