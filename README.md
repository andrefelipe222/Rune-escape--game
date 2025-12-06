<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Will: A Maga da Masmorra — Puzzle Game</title>

  <!-- Fonte retro (pixel-style) -->
  <link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&family=Inter:wght@400;600;800&display=swap" rel="stylesheet">

  <style>
    :root{
      --bg:#0e0f13;
      --panel:#171922;
      --panel-2:#1f2330;
      --accent:#ff4fd1;
      --accent-2:#6cf1ff;
      --text:#e9ecf1;
      --muted:#9aa3b2;
      --success:#7aff92;
      --warning:#ffd66c;
      --danger:#ff6c6c;
      --pixel-border: drop-shadow(0 0 0 #000) drop-shadow(0 0 0 #000);
    }

    *{ box-sizing:border-box; }
    html,body{
      height:100%;
      background: radial-gradient(1200px 600px at 15% 10%, #10131a 20%, var(--bg) 60%), #0c0d11;
      color:var(--text);
      margin:0;
      font-family: Inter, system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
      text-rendering: optimizeLegibility;
    }

    /* leve efeito “CRT” */
    body::before{
      content:"";
      position:fixed;
      inset:0;
      pointer-events:none;
      background:
        linear-gradient(rgba(255,255,255,0.025), rgba(0,0,0,0.04)),
        repeating-linear-gradient(0deg, rgba(255,255,255,0.04) 0px, rgba(255,255,255,0.04) 1px, transparent 2px, transparent 3px);
      mix-blend-mode: soft-light;
    }

    a{ color:var(--accent-2); text-decoration:none; }
    a:hover{ text-decoration:underline; }

    .container{
      max-width:1080px;
      margin:0 auto;
      padding:24px;
    }

    header.nav{
      position:sticky;
      top:0;
      z-index:10;
      background:rgba(13,15,19,0.75);
      backdrop-filter: blur(6px);
      border-bottom:1px solid #222636;
    }
    .nav-inner{
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:16px;
      padding:14px 24px;
    }
    .brand{
      display:flex; align-items:center; gap:12px;
      font-family: 'Press Start 2P', monospace;
      font-size:14px;
      letter-spacing:0.5px;
    }
    .brand .chip{
      width:28px; height:28px; background:var(--accent);
      border-radius:4px;
      box-shadow: 0 0 0 3px #2a0d20, 0 6px 24px rgba(255,79,209,0.35);
    }
    .nav-actions{
      display:flex; gap:10px;
    }
    .btn{
      display:inline-flex; align-items:center; justify-content:center;
      gap:8px; padding:10px 14px;
      border:1px solid #2a2f42;
      background:linear-gradient(180deg, #232733 0%, #1b1f2b 100%);
      color:var(--text);
      border-radius:8px;
      transition:transform .06s ease, box-shadow .2s ease, border-color .2s ease;
      font-weight:600;
    }
    .btn:hover{
      transform: translateY(-1px);
      box-shadow: 0 10px 24px rgba(0,0,0,0.35);
      border-color:#3a405b;
    }
    .btn.primary{
      border-color:#5a2d48;
      background:linear-gradient(180deg, #2a1222, #1c0b18);
      color:#ffd7f3;
      box-shadow: 0 10px 28px rgba(255,79,209,0.25) inset;
    }
    .btn.primary:hover{ border-color:#ff4fd1; }

    .hero{
      position:relative;
      border:1px solid #24293a;
      background:
        radial-gradient(1200px 600px at 80% 0%, rgba(255,79,209,0.08), transparent 40%),
        radial-gradient(900px 500px at 0% 100%, rgba(108,241,255,0.08), transparent 45%),
        linear-gradient(180deg, #191c26, #141720 60%, #121520);
      overflow:hidden;
      border-radius:16px;
      margin-top:18px;
    }
    .hero-grid{
      display:grid;
      grid-template-columns:1.2fr 1fr;
      gap:18px;
      padding:28px;
    }
    .hero h1{
      font-family:'Press Start 2P', monospace;
      font-size:22px;
      line-height:1.35;
      margin:0 0 12px;
      color:#ffeaf7;
      text-shadow: 0 0 16px rgba(255,79,209,0.25);
    }
    .tagline{
      font-size:16px;
      color:var(--muted);
      margin-bottom:18px;
    }
    .badge-row{
      display:flex; flex-wrap:wrap; gap:8px;
      margin:12px 0 18px;
    }
    .badge{
      font-size:12px; color:#d6e0ea;
      padding:6px 10px; border-radius:8px;
      background:linear-gradient(180deg, #252a3b, #202536);
      border:1px solid #31374e;
    }

    .hero-card{
      border:1px solid #2a2f42;
      background:linear-gradient(180deg, #1b1f2b, #151825);
      border-radius:12px;
      padding:16px;
    }
    .hero-card h3{
      margin:0 0 10px;
      font-size:14px; letter-spacing:0.5px;
      text-transform:uppercase; color:#b8c0cf;
    }
    .hero-preview{
      display:grid;
      grid-template-columns: 1fr 1fr;
      gap:10px;
    }
    .pixel-box{
      position:relative;
      aspect-ratio: 16 / 10;
      border:2px solid #2b1332;
      border-radius:6px;
      background:
        radial-gradient(140px 100px at 20% 20%, rgba(255,79,209,0.22), transparent 60%),
        radial-gradient(140px 100px at 80% 80%, rgba(108,241,255,0.18), transparent 60%),
        #0e0f13;
      image-rendering: pixelated;
      box-shadow: 0 0 0 4px #130a18, 0 14px 28px rgba(0,0,0,0.45);
    }
    .pixel-box::after{
      content:"preview 2D • pixel art";
      position:absolute; bottom:6px; right:8px;
      font-size:10px; color:#a9a9b6;
      background:rgba(20,22,31,0.75);
      border:1px solid #2e3244; border-radius:6px;
      padding:4px 6px;
    }

    .section{
      margin-top:28px;
      border:1px solid #222636;
      border-radius:14px;
      overflow:hidden;
      background:linear-gradient(180deg, #181b26, #141823);
    }
    .section header{
      padding:16px 18px;
      border-bottom:1px solid #22273a;
      background:linear-gradient(180deg, #1b1f2b, #171a26);
    }
    .section header h2{
      margin:0; font-size:18px; font-weight:800;
    }
    .section .content{
      padding:18px;
    }

    .grid-2{
      display:grid;
      grid-template-columns: 1fr 1fr;
      gap:16px;
    }
    .grid-3{
      display:grid;
      grid-template-columns: repeat(3, 1fr);
      gap:16px;
    }
    @media (max-width:900px){
      .hero-grid{ grid-template-columns: 1fr; }
      .grid-2{ grid-template-columns: 1fr; }
      .grid-3{ grid-template-columns: 1fr; }
    }

    .card{
      border:1px solid #24293a;
      background:linear-gradient(180deg, #1a1e2a, #151925);
      border-radius:12px;
      padding:14px;
    }
    .card h3{
      margin:0 0 10px; font-size:14px; color:#b8c0cf; text-transform:uppercase;
    }
    .card p{ margin:0; color:#dbe2ea; }

    .feature{
      display:flex; gap:12px;
      align-items:flex-start;
    }
    .icon{
      width:36px; height:36px; border-radius:8px;
      background:linear-gradient(180deg, #2a1222, #1c0b18);
      border:1px solid #3c1832;
      color:#ffd7f3;
      display:flex; align-items:center; justify-content:center;
      font-weight:800;
      box-shadow: 0 10px 28px rgba(255,79,209,0.15) inset;
    }

    .list{
      display:grid; gap:12px; margin:0; padding:0; list-style:none;
    }
    .list li{
      display:flex; gap:12px; align-items:flex-start;
      border:1px dashed #2b2f44; border-radius:10px; padding:10px 12px;
      background:linear-gradient(180deg, #171a26, #141723);
    }
    .list .k{
      min-width:40px; text-align:center;
      padding:6px 10px; border-radius:8px;
      font-family:'Press Start 2P', monospace; font-size:12px;
      color:#ffecf8; background:#2a1222; border:1px solid #3c1832;
    }

    .tag{
      display:inline-block;
      font-size:12px; color:#d6e0ea;
      padding:6px 10px; border-radius:8px;
      background:linear-gradient(180deg, #252a3b, #202536);
      border:1px solid #31374e;
      margin-right:8px;
    }

    .footer{
      margin:32px 0 12px; color:#8f98a8; text-align:center;
    }

    .muted{ color:var(--muted); }
    .accent{ color:var(--accent); }
  </style>
</head>
<body>
  <header class="nav">
    <div class="nav-inner container">
      <div class="brand">
        <div class="chip" aria-hidden="true"></div>
        <span>Will: A Maga da Masmorra</span>
      </div>
      <div class="nav-actions">
        <button class="btn">Página inicial</button>
        <button class="btn">Detalhes</button>
        <button class="btn primary">Baixar para Windows</button>
      </div>
    </div>
  </header>

  <main class="container">
    <!-- HERO -->
    <section class="hero">
      <div class="hero-grid">
        <div>
          <h1>Escape a masmorra decifrando mensagens e alinhando chaves</h1>
          <p class="tagline">
            Um puzzle 2D em pixel art com foco, calma e atenção total — sem inimigos, apenas você e os enigmas.
          </p>
          <div class="badge-row">
            <span class="badge">Gênero: Puzzle</span>
            <span class="badge">Plataforma: Windows</span>
            <span class="badge">Níveis: 4</span>
            <span class="badge">Inimigos: 0</span>
            <span class="badge">Dificuldade: Fácil</span>
          </div>
          <div style="display:flex; gap:12px; margin-top:8px;">
            <button class="btn primary">Jogar agora</button>
            <button class="btn">Ver controles</button>
          </div>
        </div>
        <div class="hero-card">
          <h3>Prévia</h3>
          <div class="hero-preview">
            <div class="pixel-box" aria-label="Tela do jogo em pixel art"></div>
            <div class="pixel-box" aria-label="Ambiente de masmorra"></div>
          </div>
        </div>
      </div>
    </section>

    <!-- IDENTIDADE E MECÂNICA -->
    <section class="section">
      <header><h2>Identidade do jogo</h2></header>
      <div class="content grid-2">
        <div class="card">
          <h3>Quem é Will</h3>
          <p>
            Will é uma maga determinada em escapar de uma masmorra. Sua jornada não envolve combate,
            e sim inteligência: ela resolve puzzles, decifra mensagens e encontra combinações secretas
            para destrancar seu caminho.
          </p>
        </div>
        <div class="card">
          <h3>Como se joga</h3>
          <p>
            O jogo é centrado em puzzles de decodificação e alinhamento de chaves. O ritmo é lento,
            favorecendo a observação e o foco do jogador. Cada fase exige atenção aos detalhes
            para descobrir a solução correta.
          </p>
        </div>
      </div>
    </section>

    <!-- CARACTERÍSTICAS E ARTE -->
    <section class="section">
      <header><h2>Características do mundo e arte</h2></header>
      <div class="content grid-3">
        <div class="card feature">
          <div class="icon">⛓</div>
          <div>
            <h3>Ambiente</h3>
            <p>
              Uma masmorra silenciosa, com atmosfera densa e iluminação mínima. A movimentação é
              propositalmente lenta para incentivar a atenção plena e a leitura do cenário.
            </p>
          </div>
        </div>
        <div class="card feature">
          <div class="icon">🟪</div>
          <div>
            <h3>Arte 2D</h3>
            <p>
              Pixel art com paleta sombria e acentos em rosa e ciano, reforçando o tema arcano de Will
              e o clima de mistério nos corredores da masmorra.
            </p>
          </div>
        </div>
        <div class="card feature">
          <div class="icon">🎵</div>
          <div>
            <h3>Trilha sonora</h3>
            <p>
              Música ambiente obtida do YouTube, criando um pano de fundo etéreo e contemplativo.
              (Garanta que tem direitos de uso para distribuição do jogo.)
            </p>
          </div>
        </div>
      </div>
    </section>

    <!-- INTERFACE / CONTROLES -->
    <section class="section">
      <header><h2>Interface e controles</h2></header>
      <div class="content">
        <ul class="list">
          <li>
            <span class="k">← →</span>
            <div>
              <strong class="accent">Movimento lateral:</strong>
              Use as setas esquerda e direita para se deslocar pelos corredores da masmorra.
            </div>
          </li>
          <li>
            <span class="k">Espaço</span>
            <div>
              <strong class="accent">Pular:</strong>
              Pequenos saltos para acessar plataformas e áreas elevadas.
            </div>
          </li>
          <li>
            <span class="k">Q</span>
            <div>
              <strong class="accent">Interação 1:</strong>
              Examinar inscrições, ler mensagens e manipular mecanismos iniciais.
            </div>
          </li>
          <li>
            <span class="k">E</span>
            <div>
              <strong class="accent">Interação 2:</strong>
              Confirmar ações, girar chaves e alinhar elementos dos puzzles.
            </div>
          </li>
        </ul>
      </div>
    </section>

    <!-- DIFICULDADE E PROGRESSÃO -->
    <section class="section">
      <header><h2>Dificuldade e progressão</h2></header>
      <div class="content grid-2">
        <div class="card">
          <h3>Desafios</h3>
          <p>
            Puzzles de dificuldade fácil, focados mais no tempo e na atenção do jogador do que em
            complexidade extrema. Não há inimigos — o desafio é mental.
          </p>
        </div>
        <div class="card">
          <h3>Estrutura</h3>
          <p>
            Campanha com 4 níveis. Cada nível apresenta novas pistas, novas combinações e variações
            de decodificação, mantendo o ritmo contemplativo.
          </p>
        </div>
      </div>
    </section>

    <!-- PERSONAGEM -->
    <section class="section">
      <header><h2>Personagem</h2></header>
      <div class="content grid-2">
        <div class="card">
          <h3>Visual de Will</h3>
          <p>
            Will é uma personagem feminina com um grande chapéu rosa e um manto também rosa.
            Seu visual se destaca na penumbra da masmorra, guiando o olhar do jogador.
          </p>
        </div>
        <div class="card">
          <h3>Presença</h3>
          <p>
            Sem combate, Will se comunica com o ambiente por meio de símbolos, engrenagens e
            fechos arcânicos — sempre em busca da combinação certa para avançar.
          </p>
        </div>
      </div>
    </section>

    <!-- FICHA TÉCNICA -->
    <section class="section">
      <header><h2>Ficha técnica</h2></header>
      <div class="content">
        <div class="grid-3">
          <div class="card">
            <h3>Gênero</h3>
            <p>Puzzle</p>
          </div>
          <div class="card">
            <h3>Plataformas</h3>
            <p>Windows</p>
          </div>
          <div class="card">
            <h3>Quantidade de níveis</h3>
            <p>4</p>
          </div>
          <div class="card">
            <h3>Vilões/Inimigos</h3>
            <p>0</p>
          </div>
          <div class="card">
            <h3>Público-alvo</h3>
            <p>Pessoas interessadas em jogos de puzzle.</p>
          </div>
          <div class="card">
            <h3>Tipos de puzzles</h3>
            <p>Decodificação de mensagens e alinhamento de chaves.</p>
          </div>
        </div>
        <div style="margin-top:14px;">
          <span class="tag">Ritmo: lento</span>
          <span class="tag">Foco: alto</span>
          <span class="tag">Exploração: masmorra</span>
          <span class="tag">Arte: 2D pixel art</span>
        </div>
      </div>
    </section>

    <!-- CHAMADA FINAL -->
    <section class="section">
      <header><h2>Baixar e jogar</h2></header>
      <div class="content grid-2">
        <div class="card">
          <h3>Windows</h3>
          <p>
            Disponível para Windows. Faça o download e mergulhe nos corredores silenciosos,
            decifrando cada mensagem para abrir o próximo portão.
          </p>
          <div style="margin-top:10px; display:flex; gap:10px;">
            <button class="btn primary">Baixar agora</button>
            <button class="btn">Ver requisitos</button>
          </div>
        </div>
        <div class="card">
          <h3>Dica</h3>
          <p class="muted">
            Use fones de ouvido e jogue sem pressa. Observe padrões, símbolos e o posicionamento
            das chaves — o caminho se abre para quem lê o ambiente.
          </p>
        </div>
      </div>
    </section>

    <p class="footer">
      © 2025 Will: A Maga da Masmorra — Página de jogo criada com base nas informações fornecidas.
    </p>
  </main>
</body>
</html>

