<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>Almanaque do Futebol de Raiz</title>
  <meta name="description" content="Recordando a época de ouro das chuteiras de couro e dos grandes mitos do futebol de raiz.">
  <link rel="icon" href="/favicon.ico">
  <!-- Open Graph (improves link previews) -->
  <meta property="og:title" content="Almanaque do Futebol de Raiz">
  <meta property="og:description" content="Recordando a época de ouro das chuteiras de couro e dos grandes mitos">
  <meta property="og:type" content="website">

  <style>
    :root{
      --bg:#f4efe6; --card:#eae3d2; --text:#2b261f; --muted:#5c5346;
      --gold:#b8860b; --border:#8c7a6b; --shadow:0 4px 10px rgba(0,0,0,.15);
      --radius:8px; --gap:28px;
    }
    *{box-sizing:border-box;font-family: Georgia, "Times New Roman", serif}
    body{background:var(--bg); color:var(--text); margin:0; padding-bottom:40px}
    header{background:var(--card); padding:40px 20px; text-align:center; border-bottom:3px double var(--border); margin-bottom:30px}
    header h1{font-size:2.2rem; letter-spacing:1px; text-transform:uppercase}
    header p{color:var(--muted); font-style:italic}

    /* Responsive grid */
    .gallery-container{
      display:grid;
      grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
      gap:var(--gap);
      max-width:1200px; margin:0 auto; padding:0 20px;
    }

    .card{background:var(--card); border:1px solid var(--border); border-radius:var(--radius);
          box-shadow:var(--shadow); overflow:hidden; display:flex; flex-direction:column; transition:transform .18s ease}
    .card:hover{transform:translateY(-4px)}

    figure{margin:0}
    img{display:block; width:100%; height:auto; max-height:220px; object-fit:cover;
        filter:grayscale(100%) sepia(30%);}

    .card-content{padding:20px; display:flex; flex-direction:column; gap:12px}
    .card-content h3{font-size:1.25rem; margin:0; padding-bottom:6px; border-bottom:1px solid var(--border)}
    .card-content p{color:var(--muted); line-height:1.5; margin:0; flex:1}

    .like-btn{
      background:#3b3228; color:var(--bg); border:1px solid var(--border);
      padding:10px 16px; border-radius:4px; cursor:pointer; font-weight:bold;
      display:inline-flex; gap:6px; align-items:center; justify-content:center;
      align-self:flex-start; font-family:inherit; font-size:.95rem;
    }
    .like-btn.liked{background:var(--gold); color:var(--text)}
    .like-btn:focus-visible{outline:3px solid rgba(184,134,11,.25); outline-offset:2px}

    @media (prefers-reduced-motion: reduce){
      *{transition:none !important}
    }

    /* Small utility */
    .sr-only{position: absolute; left:-10000px; top:auto; width:1px; height:1px; overflow:hidden}
  </style>
</head>
<body>
  <header>
    <h1 id="page-title">📜 Almanaque do Futebol de Raiz</h1>
    <p>Recordando a época de ouro das chuteiras de couro e dos grandes mitos</p>
  </header>

  <main class="gallery-container" aria-labelledby="page-title">
    <!-- Example card: use data-id to uniquely identify items for persistence -->
    <article class="card" data-id="ball">
      <figure>
        <!-- width/height reduce layout shift; use srcset for responsive sizes -->
        <img src="https://images.unsplash.com/photo-1518091043644-c1d4457512c6?w=800&q=80"
             srcset="https://images.unsplash.com/photo-1518091043644-c1d4457512c6?w=400 400w,
                     https://images.unsplash.com/photo-1518091043644-c1d4457512c6?w=800 800w"
             sizes="(max-width:420px) 100vw, 320px"
             width="800" height="440"
             loading="lazy" decoding="async"
             alt="Bola de futebol vintage de couro">
        <figcaption class="sr-only">Bola de couro vintage</figcaption>
      </figure>

      <div class="card-content">
        <h3>A Clássica Bola de Couro</h3>
        <p>Muito mais pesada e costurada a mão. Em dias de chuva, dobrava de peso e exigia pura raça dos atletas.</p>

        <div>
          <button type="button" class="like-btn" aria-pressed="false" data-id="ball" aria-label="Aplausos: 0">
            🏆 <span class="like-count" aria-live="polite">0</span> Aplausos
          </button>
        </div>
      </div>
    </article>

    <!-- Duplicate other cards similarly, each with a unique data-id -->
  </main>

  <!-- Announcement region for screen readers (keeps aria-live separate from button text) -->
  <div id="applause-status" class="sr-only" role="status" aria-live="polite"></div>

  <noscript>
    <p style="text-align:center; padding:10px">Interatividade (aplausos) requer JavaScript. A página é totalmente legível sem JS.</p>
  </noscript>

  <script>
    // Event delegation + localStorage persistence + accessible announcements
    (function(){
      const storageKey = 'sitedoluan_aplausos_v1';
      const gallery = document.querySelector('.gallery-container');
      const status = document.getElementById('applause-status');

      // Load stored counts (object keyed by data-id)
      let store = {};
      try { store = JSON.parse(localStorage.getItem(storageKey)) || {} } catch(e){ store = {} }

      // Initialize UI from DOM and store
      document.querySelectorAll('.card').forEach(card => {
        const id = card.dataset.id;
        if(!id) return;
        const btn = card.querySelector('.like-btn');
        const countEl = btn.querySelector('.like-count');
        const count = Number(store[id] || countEl.textContent || 0);
        countEl.textContent = count;
        btn.setAttribute('aria-pressed', (store['__liked_' + id] ? 'true' : 'false'));
        if(store['__liked_' + id]) btn.classList.add('liked');
        btn.setAttribute('aria-label', `Aplausos: ${count}`);
      });

      // Handle click with delegation
      gallery.addEventListener('click', (e) => {
        const btn = e.target.closest('.like-btn');
        if(!btn) return;
        const id = btn.dataset.id || btn.closest('.card')?.dataset?.id;
        if(!id) return;
        const countEl = btn.querySelector('.like-count');
        let count = Number(countEl.textContent || 0);
        const likedKey = '__liked_' + id;
        const isLiked = !!store[likedKey];

        if(isLiked){
          count = Math.max(0, count - 1);
          delete store[likedKey];
        } else {
          count = count + 1;
          store[likedKey] = true;
        }
        store[id] = count;

        // persist
        try { localStorage.setItem(storageKey, JSON.stringify(store)) } catch(e){ /* ignore */ }

        // update DOM & announce
        countEl.textContent = count;
        btn.setAttribute('aria-pressed', !!store[likedKey]);
        btn.classList.toggle('liked', !!store[likedKey]);
        btn.setAttribute('aria-label', `Aplausos: ${count}`);
        status.textContent = `Aplausos atualizados: ${count} para ${id}`;
      });

      // Optional: keyboard support handled by <button> element by default
    })();
  </script>
</body>
</html>