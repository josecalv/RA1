<!doctype html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Aula Taller · RA1 Motores y Mantenimiento</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
  <style>
    /* ── TOKENS ── */
    :root, [data-theme="light"] {
      --bg: #f7f6f2; --surface: #f9f8f5; --surface2: #fbfbf9; --surface3: #f0ece6;
      --border: #d4d1ca; --text: #28251d; --muted: #6f6d67; --inv: #faf8f5;
      --primary: #01696f; --success: #437a22; --danger: #b13b82; --gold: #d19900;
      --radius: 18px; --shadow: 0 18px 40px rgba(35,28,18,.09);
      --font-display: 'Instrument Serif', Georgia, serif;
      --font-body: 'Inter', system-ui, sans-serif;
    }
    [data-theme="dark"] {
      --bg: #171614; --surface: #1c1b19; --surface2: #201f1d; --surface3: #262422;
      --border: #393836; --text: #e2dfd9; --muted: #a5a19b; --inv: #191816;
      --primary: #4f98a3; --success: #76b14c; --danger: #db77b2; --gold: #e4b843;
      --shadow: 0 18px 40px rgba(0,0,0,.35);
    }

    /* ── RESET & BASE ── */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    body { font-family: var(--font-body); background: var(--bg); color: var(--text); line-height: 1.6; }
    .wrap { max-width: 1260px; margin: 0 auto; padding: 0 16px; }

    /* ── HEADER ── */
    header {
      position: sticky; top: 0; z-index: 50;
      background: color-mix(in srgb, var(--bg) 88%, transparent);
      backdrop-filter: blur(10px);
      border-bottom: 1px solid var(--border);
    }
    .topbar { display: flex; justify-content: space-between; gap: 16px; align-items: center; padding: 14px 0; }
    .brand { display: flex; gap: 12px; align-items: center; }
    .logo {
      width: 46px; height: 46px; border-radius: 14px; flex-shrink: 0;
      background: linear-gradient(145deg, var(--surface3), var(--surface2));
      display: grid; place-items: center; border: 1px solid var(--border);
    }
    .logo svg { width: 25px; height: 25px; color: var(--primary); }
    .brand small { display: block; color: var(--muted); text-transform: uppercase; letter-spacing: .05em; font-size: 12px; }
    .brand strong { font-size: 15px; }
    .nav { display: flex; gap: 8px; flex-wrap: wrap; align-items: center; }

    /* ── SHARED INTERACTIVE ELEMENTS ── */
    .nav a, .theme-btn, .btn, .chip, .pill {
      display: inline-flex; align-items: center; justify-content: center;
      min-height: 44px; padding: .75rem 1rem; border-radius: 999px;
      border: 1px solid var(--border); background: var(--surface);
      color: var(--text); text-decoration: none; cursor: pointer;
      font-family: var(--font-body); font-size: 14px;
      transition: opacity .15s, background .15s;
    }
    .nav a:hover, .btn:hover { opacity: .8; }
    .btn-primary { background: var(--primary); border-color: var(--primary); color: var(--inv); }
    .btn-secondary { background: var(--surface2); }

    /* ── HERO ── */
    .hero { padding: 42px 0 24px; position: relative; overflow: hidden; }
    .hero::after {
      content: ''; position: absolute; left: 50%; top: 56%;
      transform: translate(-50%, -50%);
      width: min(30vw, 360px); height: min(30vw, 360px);
      pointer-events: none; opacity: .12;
      background: url('https://pngimg.com/uploads/engine/engine_PNG23.png') no-repeat center / contain;
      filter: grayscale(1) contrast(.9) brightness(1.08);
    }
    .hero-grid {
      display: grid; grid-template-columns: minmax(0,1.12fr) minmax(320px,.88fr);
      gap: 32px; align-items: start; position: relative; z-index: 1;
    }
    .eyebrow {
      display: inline-flex; align-items: center; gap: 8px;
      padding: .4rem .75rem; border-radius: 999px;
      background: color-mix(in srgb, var(--primary) 14%, var(--surface));
      color: var(--primary); font-size: 12px; font-weight: 800;
      text-transform: uppercase; letter-spacing: .05em;
    }
    h1 { font-family: var(--font-display); font-size: clamp(2.4rem,4vw,4.4rem); line-height: 1.02; margin: 14px 0 10px; }
    h2 { font-size: clamp(1.6rem,2vw,2.3rem); line-height: 1.08; }
    h3 { font-size: 1.15rem; line-height: 1.15; }
    .lead { font-size: 1.08rem; color: var(--muted); max-width: 66ch; }

    /* ── PANEL / CARD ── */
    .panel, .quiz-box, .status-box, .report-box {
      background: var(--surface); border: 1px solid var(--border);
      border-radius: var(--radius); box-shadow: var(--shadow); padding: 20px;
    }
    .hero-side { display: grid; gap: 14px; }
    .hero-stats { display: grid; grid-template-columns: repeat(3,1fr); gap: 12px; }
    .stat { padding: 16px; border-radius: 16px; background: var(--surface2); border: 1px solid var(--border); }

    /* ── CHIPS / PILLS ── */
    .chips { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 14px; }
    .chip, .pill { min-height: auto; padding: .45rem .75rem; font-size: 12px; background: var(--surface3); }

    /* ── BADGES ── */
    .cards4 { display: grid; gap: 14px; grid-template-columns: repeat(4,1fr); }
    .badgebox { padding: 16px; border-radius: 16px; background: var(--surface2); border: 1px solid var(--border); }
    .badge-lock { opacity: .55; filter: grayscale(.25); }

    /* ── QUIZ LAYOUT ── */
    .quiz-layout { display: grid; grid-template-columns: .8fr 1.2fr; gap: 18px; align-items: start; }
    .status-box { position: sticky; top: 96px; }
    .progress { height: 14px; border-radius: 999px; background: var(--surface3); border: 1px solid var(--border); overflow: hidden; margin: 10px 0; }
    .bar { height: 100%; width: 0; background: linear-gradient(90deg, var(--primary), var(--gold)); transition: width .3s ease; }
    .meta { display: flex; flex-wrap: wrap; gap: 10px; margin: 12px 0; }

    /* ── QUESTION & OPTIONS ── */
    .question-title { margin: .4rem 0 .8rem; }
    .options { display: grid; gap: 10px; margin-top: 14px; }
    .option {
      padding: 14px 16px; border-radius: 16px;
      border: 1px solid var(--border); background: var(--surface2);
      text-align: left; min-height: 54px; color: var(--text);
      cursor: pointer; font-size: 14px; font-family: var(--font-body);
      transition: background .15s, border-color .15s;
    }
    .option:hover:not(:disabled) { background: var(--surface3); }
    .option:disabled { cursor: default; }

    /* ── FEEDBACK BOXES ── */
    .feedback, .reward, .review-box, .save-status, .gate-note {
      display: none; margin-top: 14px; padding: 14px;
      border-radius: 16px; background: var(--surface3); border: 1px solid var(--border);
    }
    .ok  { display: block; background: color-mix(in srgb,var(--success) 14%,var(--surface)); border: 1px solid color-mix(in srgb,var(--success) 38%,var(--border)); }
    .bad { display: block; background: color-mix(in srgb,var(--danger)  12%,var(--surface)); border: 1px solid color-mix(in srgb,var(--danger)  32%,var(--border)); }
    .gate-note { display: block; background: color-mix(in srgb,var(--gold) 14%,var(--surface)); border: 1px solid color-mix(in srgb,var(--gold) 35%,var(--border)); }
    .save-status { display: block; font-size: 14px; }

    /* ── STUDENT FORM ── */
    .student-form { display: grid; grid-template-columns: 1fr auto; gap: 10px; margin-top: 12px; }
    .student-form input {
      min-height: 44px; padding: .8rem 1rem; border-radius: 14px;
      border: 1px solid var(--border); background: var(--surface2);
      color: var(--text); font-size: 14px; font-family: var(--font-body); outline: none;
    }
    .student-form input:focus { border-color: var(--primary); }
    .save-box { display: flex; flex-wrap: wrap; gap: 10px; margin-top: 12px; }

    /* ── TABLE ── */
    .table { width: 100%; border-collapse: collapse; margin-top: 10px; }
    .table th, .table td { text-align: left; padding: 10px; border-bottom: 1px solid var(--border); font-size: 14px; }
    .table th { font-weight: 600; color: var(--muted); font-size: 12px; text-transform: uppercase; letter-spacing: .04em; }

    /* ── SECTION ── */
    .section { padding: 24px 0; }
    .section-head { display: flex; justify-content: space-between; align-items: end; gap: 16px; margin-bottom: 16px; }
    .muted { color: var(--muted); }

    /* ── FOOTER ── */
    footer { padding: 36px 0 60px; color: var(--muted); }

    /* ── RESPONSIVE ── */
    @media (max-width: 1080px) {
      .hero-grid, .quiz-layout, .cards4 { grid-template-columns: 1fr 1fr; }
      .status-box { position: static; }
      .hero::after { left: 50%; top: 60%; width: min(34vw,300px); height: min(34vw,300px); opacity: .095; }
    }
    @media (max-width: 760px) {
      .hero-grid, .quiz-layout, .cards4, .hero-stats, .student-form { grid-template-columns: 1fr; }
      .topbar { flex-direction: column; align-items: flex-start; }
      .hero::after { left: 50%; top: 78%; width: min(52vw,220px); height: min(52vw,220px); opacity: .075; }
    }
  </style>
</head>
<body>

<header>
  <div class="wrap topbar">
    <div class="brand">
      <div class="logo" aria-hidden="true">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
          <path d="M4 16h16"/><path d="M6 16l2-6h8l2 6"/>
          <path d="M9 10V7.5a3 3 0 0 1 6 0V10"/>
        </svg>
      </div>
      <div>
        <small>RA1 Motores y Mantenimiento</small>
        <strong>Aula Taller</strong>
      </div>
    </div>
    <nav class="nav" aria-label="Principal">
      <a href="#ud2">UD2</a><a href="#ud3">UD3</a><a href="#ud4">UD4</a>
      <a href="#ud5">UD5</a><a href="#final">Final RA1</a><a href="#informe">Informe</a>
      <button class="theme-btn" id="theme-toggle" aria-label="Cambiar tema">◐</button>
    </nav>
  </div>
</header>

<main class="wrap">

  <section class="hero">
    <div class="hero-grid">
      <div>
        <span class="eyebrow">RA1</span>
        <h1>Aula Taller</h1>
        <p class="lead">RA1: Realiza el mantenimiento básico del motor de explosión y diésel analizando sus principios de funcionamiento y justificando las actuaciones de mantenimiento requeridas.</p>
        <div class="chips">
          <span class="chip">40 preguntas por unidad</span>
          <span class="chip">Se extraen 20</span>
          <span class="chip">Sin repetir dentro del intento</span>
          <span class="chip">Guardado local</span>
        </div>
        <div class="student-form">
          <input id="student-name" type="text" placeholder="Escribe el nombre del alumno" autocomplete="off">
          <button class="btn btn-primary" id="save-student">Guardar nombre</button>
        </div>
        <div class="save-box">
          <div class="save-status" id="save-status">Sin perfil cargado</div>
          <button class="btn btn-secondary" id="resume-student">Cargar progreso</button>
          <button class="btn btn-secondary" id="clear-student">Reiniciar perfil</button>
        </div>
      </div>
      <aside class="panel hero-side">
        <h3>Panel del alumno</h3>
        <div class="hero-stats">
          <div class="stat"><strong id="global-points">0</strong><br><span class="muted">Puntos</span></div>
          <div class="stat"><strong id="global-stars">0</strong><br><span class="muted">Estrellas</span></div>
          <div class="stat"><strong id="global-streak">0</strong><br><span class="muted">Mejor racha</span></div>
          <div class="stat"><strong id="global-units">0/4</strong><br><span class="muted">UD completadas</span></div>
          <div class="stat"><strong id="global-saves">0</strong><br><span class="muted">Guardados</span></div>
        </div>
      </aside>
    </div>
  </section>

  <section class="section">
    <div class="section-head">
      <div>
        <h2>Insignias del recorrido</h2>
        <p class="muted">Cada unidad usa un banco amplio y diferente en cada intento.</p>
      </div>
    </div>
    <div class="cards4">
      <div class="badgebox badge-lock" id="badge-ud2"><h3>🔧 UD2</h3><p>Motores</p></div>
      <div class="badgebox badge-lock" id="badge-ud3"><h3>💧 UD3</h3><p>Refrigeración</p></div>
      <div class="badgebox badge-lock" id="badge-ud4"><h3>🛢️ UD4</h3><p>Lubricación</p></div>
      <div class="badgebox badge-lock" id="badge-ud5"><h3>🌬️ UD5</h3><p>Alimentación y escape</p></div>
    </div>
  </section>

  <div id="quizzes"></div>

  <section class="section" id="informe">
    <div class="section-head">
      <div>
        <h2>Informe final del alumno</h2>
        <p class="muted">Resumen por unidad, intentos y evolución.</p>
      </div>
    </div>
    <div class="report-box">
      <p><strong>Alumno:</strong> <span id="report-name">Sin nombre</span></p>
      <table class="table">
        <thead><tr><th>Bloque</th><th>Mejor resultado</th><th>Intentos</th><th>Último</th><th>Nivel</th></tr></thead>
        <tbody id="report-table"></tbody>
      </table>
      <div class="review-box" id="global-diagnosis" style="display:block;margin-top:16px">
        <strong>Diagnóstico global:</strong> Cuando el alumno complete pruebas, aquí aparecerá una valoración resumida.
      </div>
    </div>
  </section>

</main>

<footer class="wrap"><p>Aula Taller · RA1 Motores y Mantenimiento.</p></footer>

<script>
/* ═══════════════════════════════════════════════════════════════════
   CONFIGURACIÓN — todos los valores clave en un solo lugar
═══════════════════════════════════════════════════════════════════ */
const CONFIG = {
  storagePrefix:    'aula-taller-ra1-v4',
  maxAttempts:      3,
  passThreshold:    0.70,   // 70 % para superar un bloque
  goldThreshold:    0.80,   // 80 % para insignia oro
  questionsPerUnit: 20,     // preguntas extraídas del banco por intento
  basicPerUnit:     6,      // preguntas básicas (band 'b') incluidas
};

const UNIT_IDS     = ['ud2', 'ud3', 'ud4', 'ud5'];
const ALL_IDS      = ['ud2', 'ud3', 'ud4', 'ud5', 'final'];
const UNLOCK_ORDER = ['ud2', 'ud3', 'ud4', 'ud5', 'final'];

const UNLOCK_RULES = {
  ud2:   null,
  ud3:   { prev: 'ud2', label: 'Necesitas al menos 70 % en UD2 o acertar todas las básicas.' },
  ud4:   { prev: 'ud3', label: 'Necesitas al menos 70 % en UD3 o acertar todas las básicas.' },
  ud5:   { prev: 'ud4', label: 'Necesitas al menos 70 % en UD4 o acertar todas las básicas.' },
  final: { prev: 'ud5', label: 'Necesitas superar UD2, UD3, UD4 y UD5 con al menos 70 % o el 100 % de básicas de cada bloque.' },
};

const UNIT_META = {
  ud2:   ['Evaluación UD2 · Motores',               '40 preguntas en banco; se extraen 20 con dificultad gradual.'],
  ud3:   ['Evaluación UD3 · Refrigeración',         '40 preguntas en banco; se extraen 20 con dificultad gradual.'],
  ud4:   ['Evaluación UD4 · Lubricación',           '40 preguntas en banco; se extraen 20 con dificultad gradual.'],
  ud5:   ['Evaluación UD5 · Alimentación y escape', '40 preguntas en banco; se extraen 20 con dificultad gradual.'],
  final: ['Evaluación final del RA1',               '10 preguntas finales progresivas tras completar las cuatro unidades.'],
};

/* ═══════════════════════════════════════════════════════════════════
   MENSAJES DE REPASO
═══════════════════════════════════════════════════════════════════ */
const REVIEW_MESSAGES = {
  ud2: {
    elementos:       'Repasa identificación de elementos del motor y su localización.',
    funcionamiento:  'Repasa ciclo, distribución y conceptos básicos de funcionamiento.',
    mantenimiento:   'Repasa comprobaciones y operaciones básicas de mantenimiento.',
  },
  ud3: {
    funcion:         'Repasa misión y elementos del sistema de refrigeración.',
    seguridad:       'Repasa riesgos y procedimiento seguro de intervención.',
    mantenimiento:   'Repasa niveles, fugas, purgado y verificación.',
  },
  ud4: {
    aceite:          'Repasa función del aceite y control de nivel.',
    componentes:     'Repasa filtro, bomba y circuito de engrase.',
    mantenimiento:   'Repasa cambios, comprobaciones y cierre de la intervención.',
  },
  ud5: {
    alimentacion:    'Repasa recorrido del aire y combustible y sus componentes.',
    encendido:       'Repasa elementos de encendido y ayuda al arranque.',
    escape:          'Repasa función del escape y control tras la intervención.',
  },
  final: {
    integracion:     'Repasa relaciones entre sistemas del motor.',
    seguridad:       'Refuerza trabajo seguro, ordenado y verificado.',
    mantenimiento:   'Repasa comprobaciones finales y mantenimiento básico integral.',
  },
};

/* ═══════════════════════════════════════════════════════════════════
   BANCO DE PREGUNTAS  (band: 'b' básico | 'm' medio | 'a' alto)
═══════════════════════════════════════════════════════════════════ */
const QUESTION_POOLS = {
  ud2: [
    {band:'b',tag:'elementos',     level:'Básico', points:10, q:'¿Qué componente cierra la parte inferior del bloque motor y contiene el aceite lubricante?',                             options:['Cárter','Tapa de balancines','Bomba de aceite','Colector de escape'],                                  correct:0, explanation:'El cárter forma la parte inferior y contiene el aceite.'},
    {band:'b',tag:'elementos',     level:'Básico', points:10, q:'¿Qué elemento se desplaza en el interior del cilindro?',                                                                options:['Pistón','Biela','Segmento','Bulón'],                                                                  correct:0, explanation:'El pistón se desplaza en el interior del cilindro.'},
    {band:'b',tag:'funcionamiento',level:'Básico', points:10, q:'La cilindrada de un motor indica:',                                                                                     options:['El volumen total desplazado por los pistones','La presión máxima del aceite','El tamaño del radiador','La potencia del alternador'], correct:0, explanation:'La cilindrada representa el volumen desplazado por los pistones.'},
    {band:'b',tag:'funcionamiento',level:'Básico', points:10, q:'En un motor de cuatro tiempos, tras la admisión se produce la fase de:',                                                options:['Compresión','Escape','Lubricación','Refrigeración'],                                                  correct:0, explanation:'El ciclo de cuatro tiempos sigue admisión, compresión, explosión y escape.'},
    {band:'b',tag:'elementos',     level:'Básico', points:10, q:'¿Qué pieza cierra superiormente los cilindros y aloja válvulas en muchos motores?',                                     options:['Culata','Cárter','Bancada','Silencioso'],                                                             correct:0, explanation:'La culata cierra la parte superior del motor.'},
    {band:'b',tag:'elementos',     level:'Básico', points:10, q:'¿Qué elemento une el pistón con el cigüeñal?',                                                                          options:['Biela','Balancín','Termostato','Inyector'],                                                           correct:0, explanation:'La biela une pistón y cigüeñal.'},
    {band:'b',tag:'funcionamiento',level:'Básico', points:10, q:'La explosión o expansión en un motor de cuatro tiempos ocurre después de:',                                             options:['La compresión','La admisión','El escape','La lubricación'],                                          correct:0, explanation:'Tras la compresión se produce la combustión-expansión.'},
    {band:'m',tag:'funcionamiento',level:'Medio',  points:15, q:'La distribución de un motor tiene como función principal:',                                                             options:['Sincronizar válvulas y movimiento de pistones','Regular la presión de engrase','Enfriar directamente el bloque','Filtrar el combustible de retorno'], correct:0, explanation:'La distribución sincroniza válvulas y pistones.'},
    {band:'m',tag:'mantenimiento', level:'Medio',  points:15, q:'Tras sustituir una correa de servicio, la comprobación más adecuada es:',                                               options:['Verificar tensión, alineación y funcionamiento','Comprobar la presión de los neumáticos','Sustituir el aceite del motor','Purgar el circuito de frenos'], correct:0, explanation:'Hay que revisar su montaje y funcionamiento.'},
    {band:'m',tag:'funcionamiento',level:'Medio',  points:15, q:'La relación de compresión se asocia principalmente con:',                                                               options:['El proceso de combustión y el rendimiento del motor','La capacidad del cárter','El caudal del radiador','La apertura del escape'], correct:0, explanation:'Se relaciona con cómo trabaja la combustión y el rendimiento.'},
    {band:'m',tag:'elementos',     level:'Medio',  points:15, q:'En un motor de gasolina, el encendido de la mezcla lo produce:',                                                        options:['La bujía','El inyector','El calentador','La válvula EGR'],                                           correct:0, explanation:'La chispa la genera la bujía.'},
    {band:'m',tag:'mantenimiento', level:'Medio',  points:15, q:'Si durante una revisión visual aparece una fuga de aceite en el motor, lo correcto es:',                                options:['Identificar origen, reparar y comprobar después','Añadir aceite y entregar el vehículo','Limpiar la zona y no revisar más','Sustituir el filtro de aire'], correct:0, explanation:'La intervención correcta localiza, repara y verifica.'},
    {band:'m',tag:'elementos',     level:'Medio',  points:15, q:'El cigüeñal transforma principalmente:',                                                                                options:['Movimiento alternativo en movimiento giratorio','Energía eléctrica en calor','Presión hidráulica en vacío','Movimiento giratorio en refrigeración'], correct:0, explanation:'Transforma el movimiento de los pistones en giro.'},
    {band:'m',tag:'funcionamiento',level:'Medio',  points:15, q:'En la fase de escape del motor se produce:',                                                                            options:['Salida de gases quemados','Entrada de mezcla nueva','Lubricación del cárter','Apertura del termostato'], correct:0, explanation:'En escape salen los gases quemados.'},
    {band:'a',tag:'funcionamiento',level:'Alto',   points:20, q:'¿Qué diferencia caracteriza mejor a un motor de dos tiempos frente a uno de cuatro?',                                  options:['Completa el ciclo útil en menos carreras del pistón','Utiliza siempre distribución por correa','No necesita lubricación','No dispone de escape'], correct:0, explanation:'La diferencia principal está en el número de carreras para completar el ciclo.'},
    {band:'a',tag:'funcionamiento',level:'Alto',   points:20, q:'Un montaje incorrecto de la distribución puede afectar directamente a:',                                                options:['La sincronización interna del motor','La presión de inflado','La geometría de la dirección','La carga de la batería'], correct:0, explanation:'La distribución mal ajustada altera la sincronización del motor.'},
    {band:'a',tag:'mantenimiento', level:'Alto',   points:20, q:'En una intervención básica del motor también se evalúa especialmente:',                                                 options:['Orden, limpieza, precisión y seguridad','Velocidad de desmontaje','Número de herramientas usadas','Tiempo de calentamiento del motor'], correct:0, explanation:'La calidad del trabajo incluye orden, limpieza y seguridad.'},
    {band:'a',tag:'elementos',     level:'Alto',   points:20, q:'¿Qué componente transmite el giro del cigüeñal hacia el sistema de distribución en muchos motores?',                    options:['Correa o cadena de distribución','Manguito superior','Filtro de aceite','Catalizador'], correct:0, explanation:'La correa o cadena transmiten el giro a la distribución.'},
    {band:'a',tag:'mantenimiento', level:'Alto',   points:20, q:'Tras finalizar una operación de mantenimiento sobre el motor, la fase final correcta es:',                              options:['Comprobar funcionamiento y ausencia de anomalías','Limpiar herramientas y marcharse','Sustituir el líquido refrigerante siempre','Desmontar la batería'], correct:0, explanation:'Hay que verificar que todo funciona correctamente.'},
    {band:'a',tag:'mantenimiento', level:'Alto',   points:20, q:'Si al finalizar una intervención el motor presenta un ruido anómalo, la actuación profesional es:',                     options:['Detener, revisar y localizar la causa','Aumentar el régimen para comprobar','Entregar el vehículo igualmente','Añadir refrigerante'], correct:0, explanation:'Ante una anomalía debe revisarse antes de dar por válida la intervención.'},
  ],

  ud3: [
    {band:'b',tag:'funcion',      level:'Básico', points:10, q:'La función principal del sistema de refrigeración es:',                        options:['Mantener la temperatura adecuada del motor','Elevar la presión de combustible','Reducir el ruido del escape','Filtrar el aire de admisión'], correct:0, explanation:'La refrigeración controla la temperatura de trabajo del motor.'},
    {band:'b',tag:'funcion',      level:'Básico', points:10, q:'¿Qué componente disipa calor al aire exterior?',                               options:['Radiador','Filtro de aceite','Colector de escape','Volante motor'], correct:0, explanation:'El radiador cede calor al exterior.'},
    {band:'b',tag:'mantenimiento',level:'Básico', points:10, q:'Si el nivel de refrigerante está bajo, lo correcto es:',                       options:['Comprobar causa y reponer según procedimiento','Vaciar completamente el circuito','Sustituir la batería','Ajustar la correa de distribución'], correct:0, explanation:'Primero se comprueba la causa y después se repone.'},
    {band:'b',tag:'seguridad',    level:'Básico', points:10, q:'Antes de abrir un circuito de refrigeración, lo más importante es:',           options:['Asegurarse de que no esté caliente y a presión','Quitar el filtro de aire','Desconectar los inyectores','Vaciar el aceite del motor'], correct:0, explanation:'Es esencial evitar abrir el circuito en caliente.'},
    {band:'b',tag:'funcion',      level:'Básico', points:10, q:'La calefacción del habitáculo aprovecha normalmente:',                         options:['El calor del circuito de refrigeración','La presión del circuito de frenos','El vacío de admisión','La tensión del alternador'], correct:0, explanation:'La calefacción usa el calor del refrigerante.'},
    {band:'b',tag:'funcion',      level:'Básico', points:10, q:'El vaso de expansión forma parte del sistema de:',                             options:['Refrigeración','Lubricación','Escape','Alimentación'], correct:0, explanation:'Pertenece al circuito de refrigeración.'},
    {band:'b',tag:'seguridad',    level:'Básico', points:10, q:'El principal riesgo al manipular refrigeración en caliente es:',               options:['Quemadura por líquido a presión','Descarga de alta tensión','Corte de suministro de combustible','Bloqueo del volante'], correct:0, explanation:'El riesgo típico es la quemadura por proyección de líquido caliente.'},
    {band:'m',tag:'funcion',      level:'Medio',  points:15, q:'El termostato regula principalmente:',                                        options:['El paso del refrigerante según temperatura','La presión del aceite','La apertura de válvulas','La mezcla aire-combustible'], correct:0, explanation:'El termostato abre o cierra el paso según temperatura.'},
    {band:'m',tag:'mantenimiento',level:'Medio',  points:15, q:'Después de sustituir un manguito del circuito, lo correcto es:',               options:['Comprobar estanqueidad y nivel','Cambiar las bujías','Limpiar el colector de escape','Ajustar la dirección'], correct:0, explanation:'Hay que comprobar que no existan fugas y que el nivel sea correcto.'},
    {band:'m',tag:'seguridad',    level:'Medio',  points:15, q:'Abrir el tapón del vaso de expansión con el motor caliente puede provocar:',   options:['Proyección de líquido caliente y quemaduras','Fallo del alternador','Aumento de presión de neumáticos','Desgaste del embrague'], correct:0, explanation:'Existe riesgo de proyección por temperatura y presión.'},
    {band:'m',tag:'funcion',      level:'Medio',  points:15, q:'Si el sistema de refrigeración no cumple su función, el efecto más directo puede ser:', options:['Sobrecalentamiento del motor','Caída de tensión de batería','Desgaste de neumáticos','Avería del limpiaparabrisas'], correct:0, explanation:'La consecuencia más directa es el sobrecalentamiento.'},
    {band:'m',tag:'mantenimiento',level:'Medio',  points:15, q:'Una fuga externa de refrigerante suele confirmarse mediante:',                 options:['Inspección visual y comprobación de estanqueidad','Medición del espesor del disco','Comprobación del alternador','Análisis del filtro de aire'], correct:0, explanation:'La inspección y estanqueidad ayudan a localizar la fuga.'},
    {band:'m',tag:'mantenimiento',level:'Medio',  points:15, q:'Tras reponer refrigerante, es conveniente verificar:',                        options:['Nivel y posibles fugas','Calado de distribución','Holgura de válvulas','Par de apriete de ruedas'], correct:0, explanation:'La reposición debe terminar con verificación de nivel y estanqueidad.'},
    {band:'m',tag:'funcion',      level:'Medio',  points:15, q:'Un radiador obstruido afecta principalmente a:',                              options:['La capacidad de disipar calor','La lubricación del cigüeñal','La chispa de las bujías','La recarga de la batería'], correct:0, explanation:'Si el radiador no disipa bien, la temperatura aumenta.'},
    {band:'a',tag:'seguridad',    level:'Alto',   points:20, q:'En una reparación del sistema de refrigeración, la secuencia profesional correcta termina con:', options:['Verificación del funcionamiento y ausencia de fugas','Arranque prolongado sin revisar niveles','Vaciado total del aceite','Sustitución del termostato siempre'], correct:0, explanation:'Toda intervención debe cerrarse verificando funcionamiento y estanqueidad.'},
    {band:'a',tag:'funcion',      level:'Alto',   points:20, q:'La bomba de agua tiene como misión principal:',                               options:['Impulsar el refrigerante por el circuito','Filtrar partículas del aceite','Regular la chispa de encendido','Recircular gases de escape'], correct:0, explanation:'La bomba hace circular el refrigerante.'},
    {band:'a',tag:'mantenimiento',level:'Alto',   points:20, q:'Si tras una intervención el motor sigue elevando su temperatura, debe sospecharse primero de:', options:['Un problema no resuelto en el circuito o en su purgado','Un desgaste del limpiaparabrisas','Una falta de combustible','Un problema de alineación'], correct:0, explanation:'Debe revisarse el sistema intervenido y el purgado.'},
    {band:'a',tag:'seguridad',    level:'Alto',   points:20, q:'Trabajar con seguridad en refrigeración implica:',                            options:['Esperar a temperatura segura y seguir procedimiento','Abrir el circuito para liberar presión rápidamente','Rellenar sin comprobar la avería','Poner el motor a alto régimen'], correct:0, explanation:'La seguridad exige esperar y seguir el procedimiento.'},
    {band:'a',tag:'funcion',      level:'Alto',   points:20, q:'La gestión térmica en vehículos electrificados es importante porque:',        options:['Protege componentes y mantiene condiciones de trabajo','Sustituye la lubricación del motor','Elimina la necesidad de mantenimiento','Solo afecta al habitáculo'], correct:0, explanation:'También en electrificados la gestión térmica es clave.'},
    {band:'a',tag:'funcion',      level:'Alto',   points:20, q:'Una falta de circulación del refrigerante puede deberse a fallo de:',         options:['Bomba de agua','Filtro de combustible','Bujía','Catalizador'], correct:0, explanation:'La bomba de agua es responsable de la circulación.'},
  ],

  ud4: [
    {band:'b',tag:'aceite',       level:'Básico', points:10, q:'La función principal del sistema de lubricación es:',                           options:['Reducir fricción y desgaste','Bajar el nivel de combustible','Mejorar la frenada','Activar la refrigeración del habitáculo'], correct:0, explanation:'La lubricación reduce rozamiento y desgaste.'},
    {band:'b',tag:'componentes',  level:'Básico', points:10, q:'¿Qué componente retiene las impurezas del aceite?',                             options:['Filtro de aceite','Radiador','Colector de escape','Compresor'], correct:0, explanation:'El filtro retiene impurezas del aceite.'},
    {band:'b',tag:'aceite',       level:'Básico', points:10, q:'Un nivel de aceite insuficiente puede provocar:',                               options:['Desgaste prematuro del motor','Aumento del caudal de aire','Mejor combustión','Mayor capacidad del cárter'], correct:0, explanation:'La falta de aceite incrementa el desgaste.'},
    {band:'b',tag:'mantenimiento',level:'Básico', points:10, q:'La comprobación básica del nivel de aceite se realiza normalmente con:',         options:['La varilla medidora','El vaso de expansión','El manómetro de ruedas','El polímetro'], correct:0, explanation:'La varilla medidora permite comprobar el nivel.'},
    {band:'b',tag:'componentes',  level:'Básico', points:10, q:'La bomba de aceite pertenece al sistema de:',                                   options:['Lubricación','Alimentación','Refrigeración','Escape'], correct:0, explanation:'La bomba de aceite forma parte de la lubricación.'},
    {band:'b',tag:'componentes',  level:'Básico', points:10, q:'El aceite del motor circula impulsado principalmente por:',                     options:['La bomba de aceite','La bomba de agua','El alternador','El turbo'], correct:0, explanation:'La bomba de aceite impulsa el lubricante.'},
    {band:'b',tag:'aceite',       level:'Básico', points:10, q:'La falta de lubricación afecta de forma directa a:',                           options:['Las superficies en movimiento del motor','La iluminación interior','La pintura del capó','Los elevalunas'], correct:0, explanation:'Afecta directamente a las piezas en movimiento.'},
    {band:'m',tag:'aceite',       level:'Medio',  points:15, q:'Para seleccionar correctamente un aceite debe atenderse a:',                   options:['La especificación del fabricante','El color de la carrocería','La capacidad del radiador','La longitud del escape'], correct:0, explanation:'Debe seguirse siempre la especificación indicada.'},
    {band:'m',tag:'mantenimiento',level:'Medio',  points:15, q:'Después de sustituir el filtro de aceite, la comprobación más adecuada es:',   options:['Verificar nivel, presión y ausencia de fugas','Cambiar líquido refrigerante','Alinear dirección','Sustituir pastillas de freno'], correct:0, explanation:'Hay que comprobar que el sistema trabaja correctamente.'},
    {band:'m',tag:'componentes',  level:'Medio',  points:15, q:'El cárter de aceite cumple principalmente la función de:',                     options:['Alojar el aceite del motor','Disipar los gases del escape','Generar chispa de encendido','Enfriar el habitáculo'], correct:0, explanation:'El cárter almacena el aceite lubricante.'},
    {band:'m',tag:'aceite',       level:'Medio',  points:15, q:'Además de lubricar, el aceite también contribuye a:',                         options:['Ayudar a evacuar calor y proteger componentes','Regular la relación de compresión','Limpiar el filtro de aire','Accionar el embrague'], correct:0, explanation:'El aceite también protege y ayuda a evacuar calor.'},
    {band:'m',tag:'mantenimiento',level:'Medio',  points:15, q:'Si aparece una fuga tras un cambio de aceite, la actuación correcta es:',      options:['Detener, localizar la fuga y corregirla','Añadir más aceite sin revisar','Entregar el vehículo y observar','Cambiar el radiador'], correct:0, explanation:'La fuga debe corregirse antes de cerrar la intervención.'},
    {band:'m',tag:'aceite',       level:'Medio',  points:15, q:'Comprobar el nivel de aceite con demasiada pendiente puede dar lugar a:',      options:['Una lectura incorrecta','Una fuga automática','Un fallo del alternador','Una avería de distribución'], correct:0, explanation:'La medición debe hacerse en condiciones adecuadas.'},
    {band:'m',tag:'componentes',  level:'Medio',  points:15, q:'El filtro y la bomba de aceite forman parte de:',                             options:['El circuito de engrase del motor','El circuito de escape','El sistema de encendido','El sistema de admisión'], correct:0, explanation:'Ambos son componentes del circuito de engrase.'},
    {band:'a',tag:'aceite',       level:'Alto',   points:20, q:'Utilizar un aceite no adecuado puede afectar a:',                             options:['La protección y el funcionamiento del motor','El equilibrado de ruedas','La capacidad del depósito','La geometría de suspensión'], correct:0, explanation:'Un aceite incorrecto compromete protección y funcionamiento.'},
    {band:'a',tag:'mantenimiento',level:'Alto',   points:20, q:'En una intervención de engrase se evalúa también:',                           options:['Orden, limpieza y seguridad en el trabajo','Velocidad del vaciado exclusivamente','Nivel de combustible del depósito','Presión del aire acondicionado'], correct:0, explanation:'La forma de trabajar también se valora.'},
    {band:'a',tag:'componentes',  level:'Alto',   points:20, q:'Un filtro de aceite obstruido compromete principalmente:',                    options:['La correcta circulación y limpieza del lubricante','La temperatura del aire de admisión','La recarga de batería','La geometría del eje delantero'], correct:0, explanation:'El filtro obstruido afecta al circuito del aceite.'},
    {band:'a',tag:'mantenimiento',level:'Alto',   points:20, q:'Tras una operación de mantenimiento de lubricación, lo profesional es:',      options:['Cerrar el trabajo verificando nivel y funcionamiento','Dar por finalizado tras rellenar aceite','Sustituir siempre el termostato','Purgar el circuito de frenos'], correct:0, explanation:'La operación termina con verificación final.'},
    {band:'a',tag:'aceite',       level:'Alto',   points:20, q:'Si el testigo de presión de aceite permanece encendido tras arrancar, debe:', options:['Pararse el motor y revisarse la causa','Circular para comprobar si se apaga','Subirse el régimen del motor','Rellenarse el vaso de expansión'], correct:0, explanation:'Un aviso de presión de aceite exige revisión inmediata.'},
    {band:'a',tag:'mantenimiento',level:'Alto',   points:20, q:'Si después del mantenimiento se detecta un ruido anómalo de falta de engrase, corresponde:', options:['Detener el motor y revisar el circuito de lubricación','Aumentar la temperatura del motor','Revisar el filtro de habitáculo','Purgar el radiador'], correct:0, explanation:'Un síntoma de falta de engrase obliga a detener y revisar.'},
  ],

  ud5: [
    {band:'b',tag:'alimentacion', level:'Básico', points:10, q:'La misión del sistema de alimentación es:',                                    options:['Suministrar aire y/o combustible al motor en las condiciones adecuadas','Reducir la temperatura del refrigerante','Impulsar aceite al motor','Disminuir el ruido de rodadura'], correct:0, explanation:'La alimentación suministra los elementos necesarios para la combustión.'},
    {band:'b',tag:'encendido',    level:'Básico', points:10, q:'En motores de gasolina, la ignición de la mezcla se realiza mediante:',        options:['Bujías','Calentadores','Inyectores-bomba','Termostatos'], correct:0, explanation:'En gasolina la mezcla se inflama con la chispa de la bujía.'},
    {band:'b',tag:'encendido',    level:'Básico', points:10, q:'En motores diésel, los calentadores ayudan especialmente en:',                options:['El arranque en frío','La lubricación del motor','El filtrado del combustible','La evacuación de gases'], correct:0, explanation:'Los calentadores favorecen el arranque en frío.'},
    {band:'b',tag:'escape',       level:'Básico', points:10, q:'El sistema de escape tiene como función principal:',                          options:['Evacuar gases de la combustión','Distribuir aceite a presión','Regular el caudal de refrigerante','Accionar la distribución'], correct:0, explanation:'El escape conduce y evacúa los gases.'},
    {band:'b',tag:'alimentacion', level:'Básico', points:10, q:'El filtro de aire pertenece al sistema de:',                                  options:['Admisión y alimentación','Lubricación','Refrigeración','Transmisión'], correct:0, explanation:'El filtro de aire forma parte de la admisión del motor.'},
    {band:'b',tag:'alimentacion', level:'Básico', points:10, q:'El recorrido del aire hacia el motor comienza normalmente por:',              options:['El sistema de admisión con su filtro de aire','El cárter de aceite','El radiador de refrigeración','El silencioso trasero'], correct:0, explanation:'El aire entra por el sistema de admisión.'},
    {band:'b',tag:'escape',       level:'Básico', points:10, q:'El colector de escape se encarga de:',                                        options:['Recoger gases de los cilindros para conducirlos al escape','Distribuir el aceite por el bloque','Alojar el filtro de aire','Enfriar el líquido refrigerante'], correct:0, explanation:'El colector reúne y conduce los gases hacia el escape.'},
    {band:'m',tag:'alimentacion', level:'Medio',  points:15, q:'Sustituir un filtro de aire obstruido ayuda a:',                             options:['Mantener una admisión correcta del motor','Elevar la presión del aceite','Refrigerar el radiador','Reducir el recorrido del pistón'], correct:0, explanation:'Un filtro limpio mejora la entrada de aire.'},
    {band:'m',tag:'escape',       level:'Medio',  points:15, q:'Tras sustituir un elemento del escape, debe comprobarse:',                   options:['Que no existan fugas ni ruidos anómalos','La tensión de la correa auxiliar','El nivel de aceite del cambio','El reglaje de válvulas'], correct:0, explanation:'Hay que verificar estanqueidad y funcionamiento.'},
    {band:'m',tag:'encendido',    level:'Medio',  points:15, q:'Una bujía en mal estado puede provocar principalmente:',                     options:['Fallo de encendido en un motor gasolina','Sobrepresión del circuito de refrigeración','Pérdida de aceite por el cárter','Desgaste de la bomba de agua'], correct:0, explanation:'Una bujía deteriorada afecta al encendido.'},
    {band:'m',tag:'alimentacion', level:'Medio',  points:15, q:'La inyección diésel pertenece al sistema de:',                               options:['Alimentación','Refrigeración','Lubricación','Distribución'], correct:0, explanation:'La inyección diésel forma parte del sistema de alimentación.'},
    {band:'m',tag:'alimentacion', level:'Medio',  points:15, q:'La sobrealimentación se relaciona con:',                                     options:['La mejora de entrada de aire al motor','La elevación del nivel de aceite','La evacuación del refrigerante','La lubricación del árbol de levas'], correct:0, explanation:'La sobrealimentación aumenta la entrada de aire al motor.'},
    {band:'m',tag:'encendido',    level:'Medio',  points:15, q:'En un diésel, el calentador actúa como ayuda a:',                            options:['La combustión en el arranque en frío','La filtración del aceite','La circulación del refrigerante','La apertura de válvulas'], correct:0, explanation:'Su función principal es facilitar el arranque en frío.'},
    {band:'m',tag:'mantenimiento',level:'Medio',  points:15, q:'Al terminar una intervención en filtros o escape, una buena práctica es:',   options:['Realizar verificación final antes de entregar el vehículo','Anotar solo el tiempo empleado','Revisar la geometría del tren delantero','Cambiar el líquido de frenos'], correct:0, explanation:'La intervención debe cerrarse con comprobación final.'},
    {band:'a',tag:'mantenimiento',level:'Alto',   points:20, q:'En mantenimiento básico de alimentación, tras sustituir un componente se debe:', options:['Verificar funcionamiento y ausencia de fallos','Cambiar la bomba de agua','Purgar siempre el circuito de frenos','Regular el termostato'], correct:0, explanation:'Debe comprobarse que el sistema funcione correctamente.'},
    {band:'a',tag:'escape',       level:'Alto',   points:20, q:'Una fuga en el sistema de escape puede detectarse por:',                     options:['Ruido anómalo o salida incorrecta de gases','Bajada del nivel de aceite del motor','Color del refrigerante','Desgaste del filtro de habitáculo'], correct:0, explanation:'Las fugas de escape suelen manifestarse por ruido o salida de gases.'},
    {band:'a',tag:'encendido',    level:'Alto',   points:20, q:'Si tras una sustitución de bujías el motor no trabaja correctamente, procede:', options:['Revisar montaje y funcionamiento del sistema de encendido','Cambiar el radiador','Rellenar el cárter sin comprobar','Sustituir el silencioso'], correct:0, explanation:'Hay que revisar el sistema intervenido antes de cerrar la reparación.'},
    {band:'a',tag:'alimentacion', level:'Alto',   points:20, q:'Trabajar con profesionalidad en este bloque implica:',                       options:['Seguir procedimiento, verificar y mantener seguridad','Sustituir piezas sin diagnóstico','Probar a alto régimen sin control','Cerrar la operación sin comprobaciones'], correct:0, explanation:'También aquí se valora orden, seguridad y verificación.'},
    {band:'a',tag:'escape',       level:'Alto',   points:20, q:'Tras la sustitución de un tramo de escape, la comprobación final más adecuada es:', options:['Comprobar fijación, estanqueidad y ausencia de ruidos','Cambiar aceite del motor','Revisar alternador','Ajustar correa de distribución'], correct:0, explanation:'La revisión final debe centrarse en su fijación y estanqueidad.'},
    {band:'a',tag:'alimentacion', level:'Alto',   points:20, q:'Si un filtro de aire está muy obstruido, lo esperable es:',                  options:['Un funcionamiento deficiente por mala admisión','Una fuga de refrigerante','Caída de presión del aceite','Desajuste de distribución'], correct:0, explanation:'La obstrucción dificulta la entrada correcta de aire.'},
  ],

  final: [
    {band:'b',tag:'integracion',  level:'Nivel 1',  points:10, q:'¿Qué sistema reduce la fricción entre piezas móviles del motor?',           options:['Lubricación','Escape','Admisión','Refrigeración habitáculo'], correct:0, explanation:'La lubricación reduce la fricción.'},
    {band:'b',tag:'integracion',  level:'Nivel 2',  points:10, q:'¿Qué elemento ayuda al arranque en frío en un motor diésel?',               options:['Calentador','Bujía de gasolina','Termostato','Filtro de aire'], correct:0, explanation:'El calentador ayuda al arranque en frío.'},
    {band:'b',tag:'integracion',  level:'Nivel 3',  points:10, q:'La cilindrada se define como:',                                             options:['El volumen desplazado por los pistones','La presión del circuito de aceite','La temperatura del refrigerante','La capacidad del depósito'], correct:0, explanation:'La cilindrada es el volumen total desplazado por los pistones.'},
    {band:'m',tag:'mantenimiento',level:'Nivel 4',  points:15, q:'Tras una intervención en refrigeración, la comprobación más importante es:', options:['Ausencia de fugas y funcionamiento correcto','Estado del filtro de habitáculo','Geometría de dirección','Alineación del escape'], correct:0, explanation:'Hay que verificar estanqueidad y funcionamiento.'},
    {band:'m',tag:'mantenimiento',level:'Nivel 5',  points:15, q:'El filtro de aceite sirve para:',                                           options:['Retener impurezas del lubricante','Enfriar el motor','Regular la admisión','Reducir el ruido del escape'], correct:0, explanation:'Su misión es retener impurezas del lubricante.'},
    {band:'m',tag:'integracion',  level:'Nivel 6',  points:15, q:'Circular con poco refrigerante puede provocar:',                            options:['Sobrecalentamiento','Falta de chispa en bujías','Desgaste del filtro de aire','Holgura en suspensión'], correct:0, explanation:'La falta de refrigerante puede causar sobrecalentamiento.'},
    {band:'a',tag:'mantenimiento',level:'Nivel 7',  points:20, q:'¿Qué operación pertenece claramente al mantenimiento básico del engrase?',  options:['Cambio del filtro de aceite','Sustitución del radiador','Purgado del embrague','Cambio del silencioso'], correct:0, explanation:'Es una operación propia del engrase.'},
    {band:'a',tag:'mantenimiento',level:'Nivel 8',  points:20, q:'Al montar una correa de servicio, debe comprobarse especialmente:',         options:['Ajuste, alineación y funcionamiento','Nivel del líquido de frenos','Par de apriete de culata','Presión del circuito de combustible'], correct:0, explanation:'La correa debe quedar bien montada y comprobada.'},
    {band:'a',tag:'seguridad',    level:'Nivel 9',  points:20, q:'¿Por qué no debe abrirse el circuito de refrigeración en caliente?',        options:['Por riesgo de proyección de líquido a presión y quemaduras','Porque se descarga la batería','Porque se altera la cilindrada','Porque se vacía el aceite'], correct:0, explanation:'Existe riesgo por presión y temperatura elevadas.'},
    {band:'a',tag:'seguridad',    level:'Nivel 10', points:25, q:'En el conjunto del RA1 se valora especialmente que el alumno trabaje con:', options:['Orden, limpieza, precisión y seguridad','Rapidez aunque no verifique','Sustitución de piezas por ensayo','Intervención sin procedimiento'], correct:0, explanation:'El criterio profesional incluye orden, limpieza, precisión y seguridad.'},
  ],
};

/* ═══════════════════════════════════════════════════════════════════
   ESTADO DE LA APLICACIÓN
═══════════════════════════════════════════════════════════════════ */
const student = { name: 'Sin nombre' };

const globalState = {
  points:         0,
  stars:          0,
  bestStreak:     0,
  completedUnits: new Set(),
  results:        {},
  saveCount:      0,
};

function defaultResult(total = CONFIG.questionsPerUnit) {
  return { bestScore: 0, lastScore: 0, total, attempts: 0, stars: 0,
           basicCorrect: 0, basicTotal: CONFIG.basicPerUnit, unlocked: false };
}

function hydrateResults() {
  ALL_IDS.forEach(id => {
    globalState.results[id] = { ...defaultResult(), ...(globalState.results[id] || {}) };
  });
}

/* ═══════════════════════════════════════════════════════════════════
   PERSISTENCIA
═══════════════════════════════════════════════════════════════════ */
function profileKey(name) {
  return `${CONFIG.storagePrefix}::${(name || '').trim().toLowerCase()}`;
}

function serializableState() {
  return {
    studentName:    student.name,
    points:         globalState.points,
    stars:          globalState.stars,
    bestStreak:     globalState.bestStreak,
    completedUnits: [...globalState.completedUnits],
    results:        globalState.results,
    saveCount:      globalState.saveCount,
  };
}

function saveProfile() {
  if (!student.name || student.name === 'Sin nombre') return;
  try {
    localStorage.setItem(profileKey(student.name), JSON.stringify(serializableState()));
    globalState.saveCount += 1;
    setStatus(`Progreso guardado para ${student.name}`);
    document.getElementById('global-saves').textContent = globalState.saveCount;
  } catch (e) {
    setStatus('No se pudo guardar (almacenamiento lleno o bloqueado).');
    console.warn('saveProfile error:', e);
  }
}

function loadProfileByName(name) {
  try {
    const raw = localStorage.getItem(profileKey(name));
    if (!raw) return false;
    const data = JSON.parse(raw);
    student.name               = data.studentName  || name || 'Sin nombre';
    globalState.points         = data.points        || 0;
    globalState.stars          = data.stars         || 0;
    globalState.bestStreak     = data.bestStreak    || 0;
    globalState.completedUnits = new Set(data.completedUnits || []);
    globalState.results        = data.results       || {};
    globalState.saveCount      = data.saveCount     || 0;
    hydrateResults();
    document.getElementById('student-name').value       = student.name;
    document.getElementById('report-name').textContent  = student.name;
    document.getElementById('global-saves').textContent = globalState.saveCount;
    setStatus(`Perfil cargado: ${student.name}`);
    return true;
  } catch (e) {
    console.warn('loadProfileByName error:', e);
    return false;
  }
}

function resetProfile() {
  try {
    if (student.name && student.name !== 'Sin nombre') localStorage.removeItem(profileKey(student.name));
  } catch (e) { /* ignorar */ }
  student.name               = 'Sin nombre';
  globalState.points         = 0;
  globalState.stars          = 0;
  globalState.bestStreak     = 0;
  globalState.completedUnits = new Set();
  globalState.results        = {};
  globalState.saveCount      = 0;
  hydrateResults();
  document.getElementById('student-name').value       = '';
  document.getElementById('report-name').textContent  = 'Sin nombre';
  document.getElementById('global-saves').textContent = '0';
  setStatus('Perfil reiniciado');
}

function setStatus(msg) {
  document.getElementById('save-status').textContent = msg;
}

/* ═══════════════════════════════════════════════════════════════════
   TEMA
═══════════════════════════════════════════════════════════════════ */
(function initTheme() {
  const root  = document.documentElement;
  const btn   = document.getElementById('theme-toggle');
  let theme   = window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';
  root.setAttribute('data-theme', theme);
  btn.addEventListener('click', () => {
    theme = (theme === 'dark') ? 'light' : 'dark';
    root.setAttribute('data-theme', theme);
  });
})();

/* ═══════════════════════════════════════════════════════════════════
   EVENTOS DEL FORMULARIO DE ALUMNO
═══════════════════════════════════════════════════════════════════ */
document.getElementById('save-student').addEventListener('click', () => {
  const v = document.getElementById('student-name').value.trim();
  student.name = v || 'Sin nombre';
  document.getElementById('report-name').textContent = student.name;
  hydrateResults();
  saveProfile();
  updateGlobalPanel();
});

document.getElementById('resume-student').addEventListener('click', () => {
  const v = document.getElementById('student-name').value.trim();
  if (!v) { setStatus('Escribe el nombre para cargar el progreso'); return; }
  if (!loadProfileByName(v)) setStatus('No existe progreso guardado con ese nombre');
  updateGlobalPanel();
  refreshAll();
});

document.getElementById('clear-student').addEventListener('click', () => {
  resetProfile();
  updateGlobalPanel();
  refreshAll();
});

/* ═══════════════════════════════════════════════════════════════════
   UTILIDADES DE PREGUNTAS
═══════════════════════════════════════════════════════════════════ */
function shuffle(arr) {
  const a = [...arr];
  for (let i = a.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]];
  }
  return a;
}

function buildSelection(pool) {
  /* Estratificamos por dificultad para garantizar al menos CONFIG.basicPerUnit básicas.
     Con bancos de 7b/7m/6a tomamos: 6 básicas + 7 medias + 6 altas = 19 → completamos
     hasta questionsPerUnit (20) con cualquier sobrante mezclado. */
  const basics  = shuffle(pool.filter(q => q.band === 'b'));
  const mids    = shuffle(pool.filter(q => q.band === 'm'));
  const highs   = shuffle(pool.filter(q => q.band === 'a'));

  const selected = [
    ...basics.slice(0, CONFIG.basicPerUnit),           // 6 básicas garantizadas
    ...mids,                                            // todas las medias disponibles
    ...highs,                                           // todas las altas disponibles
  ];

  /* Si faltan preguntas para llegar a questionsPerUnit, añadimos básicas sobrantes */
  const remaining = pool.filter(q => !selected.includes(q));
  const final = [...selected, ...shuffle(remaining)].slice(0, CONFIG.questionsPerUnit);
  return shuffle(final);
}

/* ═══════════════════════════════════════════════════════════════════
   LÓGICA DE DESBLOQUEO
═══════════════════════════════════════════════════════════════════ */
function passesUnlock(id) {
  const r        = globalState.results[id] || defaultResult();
  const minScore = Math.ceil((r.total || CONFIG.questionsPerUnit) * CONFIG.passThreshold);
  const okScore  = (r.bestScore   || 0) >= minScore;
  /* okBasic: acertó todas las básicas del intento Y al menos las de CONFIG.basicPerUnit */
  const savedTotal   = r.basicTotal   || CONFIG.basicPerUnit;
  const savedCorrect = r.basicCorrect || 0;
  const okBasic  = savedCorrect > 0 && savedCorrect >= savedTotal;
  return okScore || okBasic || !!r.unlocked;
}

function isQuizUnlocked(id) {
  if (id === 'ud2')   return true;
  if (id === 'final') return UNIT_IDS.every(uid => passesUnlock(uid));
  const rule = UNLOCK_RULES[id];
  return !rule || passesUnlock(rule.prev);
}

function gateMessage(id) {
  if (id === 'ud2')   return 'Bloque inicial disponible.';
  if (id === 'final') return 'Desbloquea esta prueba logrando al menos 70 % o 100 % de básicas en todas las unidades.';
  return UNLOCK_RULES[id]?.label || 'Disponible';
}

function attemptsLeft(id) {
  return Math.max(0, CONFIG.maxAttempts - (globalState.results[id]?.attempts || 0));
}

/* ═══════════════════════════════════════════════════════════════════
   PANEL GLOBAL Y REPORT
═══════════════════════════════════════════════════════════════════ */
function updateGlobalPanel() {
  document.getElementById('global-points').textContent = globalState.points;
  document.getElementById('global-stars').textContent  = globalState.stars;
  document.getElementById('global-streak').textContent = globalState.bestStreak;
  document.getElementById('global-units').textContent  = `${globalState.completedUnits.size}/4`;
  document.getElementById('global-saves').textContent  = globalState.saveCount;

  if (isQuizUnlocked('final')) {
    const el = document.getElementById('level-final');
    const qe = document.getElementById('question-final');
    if (el) el.textContent = 'Desbloqueada';
    if (qe) qe.textContent = 'La prueba final ya está desbloqueada. Puedes comenzar.';
  }

  updateReport();
}

function unlockBadge(id, score) {
  const badge = document.getElementById('badge-' + id);
  if (!badge) return;
  badge.classList.remove('badge-lock');
  const isGold = score / CONFIG.questionsPerUnit >= CONFIG.goldThreshold;
  badge.innerHTML = `<h3>${isGold ? '🏆' : '✅'} ${id.toUpperCase()}</h3><p>${isGold ? 'Insignia oro conseguida' : 'Unidad completada'}</p>`;
}

function levelLabel(score, total) {
  const r = score / total;
  if (r >= CONFIG.goldThreshold)  return 'Alto';
  if (r >= CONFIG.passThreshold)  return 'Medio';
  if (r > 0)                      return 'Bajo';
  return 'Pendiente';
}

function updateReport() {
  document.getElementById('report-name').textContent = student.name;

  document.getElementById('report-table').innerHTML = ALL_IDS.map(id => {
    const r     = globalState.results[id] || defaultResult();
    const label = id === 'final' ? 'Final RA1' : id.toUpperCase();
    const nivel = r.attempts ? levelLabel(r.bestScore || 0, r.total || CONFIG.questionsPerUnit) : 'Pendiente';
    return `<tr>
      <td>${label}</td>
      <td>${r.bestScore || 0}/${r.total || CONFIG.questionsPerUnit}</td>
      <td>${r.attempts  || 0}</td>
      <td>${r.lastScore || 0}/${r.total || CONFIG.questionsPerUnit}</td>
      <td>${nivel}</td>
    </tr>`;
  }).join('');

  const completed = ALL_IDS.map(id => globalState.results[id]).filter(r => r && r.attempts);
  if (!completed.length) {
    document.getElementById('global-diagnosis').innerHTML =
      '<strong>Diagnóstico global:</strong> Cuando el alumno complete pruebas, aquí aparecerá una valoración resumida.';
    return;
  }

  const avg = completed.reduce((acc, r) => acc + (r.bestScore || 0) / (r.total || CONFIG.questionsPerUnit), 0) / completed.length;
  let msg = 'Necesita refuerzo general en varios bloques del RA1.';
  if (avg >= CONFIG.goldThreshold)  msg = 'Buen dominio global del RA1 y capacidad de aplicar procedimientos básicos con seguridad.';
  else if (avg >= CONFIG.passThreshold) msg = 'Progreso adecuado, aunque conviene reforzar algunos contenidos.';
  document.getElementById('global-diagnosis').innerHTML = `<strong>Diagnóstico global:</strong> ${msg}`;
}

/* ═══════════════════════════════════════════════════════════════════
   MENSAJES DE REPASO Y RESUMEN DE PUERTA
═══════════════════════════════════════════════════════════════════ */
function buildReviewMessage(id, tagErrors) {
  const source = REVIEW_MESSAGES[id] || {};
  const sorted = Object.entries(tagErrors)
    .filter(([, count]) => count > 0)
    .sort(([, a], [, b]) => b - a)
    .slice(0, 3);
  if (!sorted.length) return '<strong>Repaso recomendado:</strong> Muy buen trabajo. Puedes pasar al siguiente bloque.';
  const items = sorted.map(([tag]) => `<li>${source[tag] || 'Repasa los contenidos de este bloque.'}</li>`).join('');
  return `<strong>Repaso recomendado:</strong><ul>${items}</ul>`;
}

function buildGateSummary(id, basicCorrect, basicTotal, finalScore, total) {
  const okScore   = finalScore >= Math.ceil(total * CONFIG.passThreshold);
  const okBasic   = basicCorrect >= basicTotal;
  const nextIndex = UNLOCK_ORDER.indexOf(id) + 1;
  const nextId    = UNLOCK_ORDER[nextIndex];
  const nextLabel = nextId === 'final' ? 'la prueba final' : (nextId || '').toUpperCase();

  if (okScore || okBasic) {
    if (!nextId) return '<strong>Bloque superado.</strong> Has cumplido el criterio de cierre del recorrido.';
    return `<strong>Bloque superado.</strong> Has conseguido ${finalScore}/${total} y ${basicCorrect}/${basicTotal} básicas. Ya puedes pasar a ${nextLabel}.`;
  }
  return `<strong>Bloque no superado.</strong> Has conseguido ${finalScore}/${total} y ${basicCorrect}/${basicTotal} básicas. Necesitas llegar al 70 % o acertar todas las preguntas básicas para desbloquear el siguiente bloque.`;
}

/* ═══════════════════════════════════════════════════════════════════
   RENDERIZADO DE SECCIONES
═══════════════════════════════════════════════════════════════════ */
function renderQuizSections() {
  document.getElementById('quizzes').innerHTML = ALL_IDS.map(id => {
    const isFinal          = id === 'final';
    const [title, sub]     = UNIT_META[id];
    const extraPills       = !isFinal
      ? `<span class="pill">🔥 <span id="streak-${id}">0</span></span>
         <span class="pill">🪙 <span id="points-${id}">0</span></span>`
      : '';
    const initialProgress  = isFinal ? 'Bloqueada hasta completar UD2, UD3, UD4 y UD5' : 'Pregunta 1 de 10';
    const initialQuestion  = isFinal ? 'Completa primero las cuatro unidades para desbloquear esta prueba final.' : '';

    return `
      <section class="section" id="${id}">
        <div class="section-head">
          <div><h2>${title}</h2><p class="muted">${sub}</p></div>
        </div>
        <div class="quiz-layout">
          <aside class="status-box">
            <h3>Estado ${isFinal ? 'final' : id.toUpperCase()}</h3>
            <div class="progress"><div class="bar" id="bar-${id}"></div></div>
            <p id="progress-${id}">${initialProgress}</p>
            <div class="meta">
              <span class="pill">⭐ <span id="score-${id}">0</span>/10</span>
              ${extraPills}
            </div>
            <button class="btn ${isFinal ? 'btn-primary' : 'btn-secondary'}" id="restart-${id}" style="display:none">
              Nuevo intento ${isFinal ? 'final' : id.toUpperCase()}
            </button>
          </aside>
          <div class="quiz-box">
            <div class="pill" id="level-${id}">${isFinal ? 'Bloqueada' : 'Básico'}</div>
            <h3 class="question-title" id="question-${id}">${initialQuestion}</h3>
            <div class="options"    id="options-${id}"></div>
            <div class="feedback"   id="feedback-${id}"></div>
            <div class="reward"     id="reward-${id}"></div>
            <div class="gate-note"  id="gate-${id}"></div>
            <div class="review-box" id="review-${id}"></div>
          </div>
        </div>
      </section>`;
  }).join('');
}

/* ═══════════════════════════════════════════════════════════════════
   QUIZ  —  lógica por bloque
═══════════════════════════════════════════════════════════════════ */
function createQuiz(id) {

  /* Referencias al DOM (evaluadas perezosamente) */
  const el = name => document.getElementById(name + '-' + id);

  /* Estado del intento */
  let questions     = [];
  let shuffledOpts  = [];
  let current       = 0;
  let score         = 0;
  let streak        = 0;
  let attemptPoints = 0;
  let tagErrors     = {};
  let basicCorrect  = 0;
  let basicTotal    = 0;
  let quizFinished  = false;   /* true cuando finishQuiz() ha sido llamado */

  function prepare() {
    questions     = (id === 'final') ? [...QUESTION_POOLS[id]] : buildSelection(QUESTION_POOLS[id]);
    current       = 0;
    score         = 0;
    streak        = 0;
    attemptPoints = 0;
    tagErrors     = {};
    basicCorrect  = 0;
    basicTotal    = questions.filter(q => q.band === 'b').length;
    quizFinished  = false;
  }

  /* ── Estado: bloqueado ── */
  function renderLocked() {
    el('options').innerHTML       = '';
    el('feedback').style.display  = 'none';
    el('reward').style.display    = 'none';
    el('review').style.display    = 'none';
    el('restart').style.display   = 'none';
    el('bar').style.width         = '0%';
    el('progress').textContent    = gateMessage(id);
    el('level').textContent       = 'Bloqueada';
    el('question').textContent    = id === 'final'
      ? 'Completa los bloques anteriores para acceder a la prueba final.'
      : 'Primero debes superar el bloque anterior para continuar.';
    const gate = el('gate');
    gate.className    = 'gate-note';
    gate.style.display= 'block';
    gate.innerHTML    = `<strong>Bloque bloqueado.</strong> ${gateMessage(id)}`;
  }

  /* ── Estado: sin intentos ── */
  function renderNoAttempts() {
    el('options').innerHTML       = '';
    el('feedback').style.display  = 'none';
    el('reward').style.display    = 'none';
    el('review').style.display    = 'none';
    el('restart').style.display   = 'none';
    el('level').textContent       = 'Sin intentos';
    el('progress').textContent    = `Has usado los ${CONFIG.maxAttempts} intentos disponibles.`;
    el('question').textContent    = 'Has agotado todos los intentos para este bloque.';
    const gate = el('gate');
    gate.className    = 'gate-note';
    gate.style.display= 'block';
    gate.innerHTML    = `<strong>Sin intentos disponibles.</strong> Máximo de ${CONFIG.maxAttempts} intentos alcanzado.`;
  }

  /* ── Renderizar pregunta ── */
  function renderQuestion() {
    if (!isQuizUnlocked(id))  { renderLocked();     return; }
    if (!attemptsLeft(id))    { renderNoAttempts(); return; }

    /* Si el intento ya terminó, no sobreescribir la pantalla de resultados */
    if (quizFinished) return;

    el('gate').style.display      = 'none';
    el('feedback').className      = 'feedback';
    el('feedback').style.display  = 'none';
    el('reward').style.display    = 'none';
    el('review').style.display    = 'none';
    el('restart').style.display   = 'none';

    const item = questions[current];
    el('progress').textContent = `Pregunta ${current + 1} de ${questions.length} · Intentos disponibles: ${attemptsLeft(id)}/${CONFIG.maxAttempts}`;
    el('bar').style.width      = `${(current / questions.length) * 100}%`;
    el('level').textContent    = item.level;
    el('question').textContent = item.q;

    shuffledOpts = shuffle(item.options.map((text, i) => ({ text, originalIndex: i })));
    el('options').innerHTML    = '';
    shuffledOpts.forEach((opt, i) => {
      const btn       = document.createElement('button');
      btn.className   = 'option';
      btn.textContent = opt.text;
      btn.addEventListener('click', () => handleAnswer(i));
      el('options').appendChild(btn);
    });
  }

  /* ── Procesar respuesta ── */
  function handleAnswer(selectedIndex) {
    const item           = questions[current];
    const correctIndex   = shuffledOpts.findIndex(o => o.originalIndex === item.correct);
    const isCorrect      = selectedIndex === correctIndex;

    [...el('options').querySelectorAll('.option')].forEach((btn, i) => {
      btn.disabled = true;
      if (i === correctIndex) {
        btn.style.borderColor = 'var(--success)';
        btn.style.background  = 'color-mix(in srgb,var(--success) 16%, var(--surface))';
      }
      if (i === selectedIndex && !isCorrect) {
        btn.style.borderColor = 'var(--danger)';
        btn.style.background  = 'color-mix(in srgb,var(--danger) 12%, var(--surface))';
      }
    });

    if (isCorrect) {
      score++;
      streak++;
      attemptPoints        += item.points;
      globalState.points   += item.points;
      if (item.band === 'b') basicCorrect++;
      if (streak > globalState.bestStreak) globalState.bestStreak = streak;
    } else {
      streak = 0;
      tagErrors[item.tag] = (tagErrors[item.tag] || 0) + 1;
    }

    el('score').textContent = score;
    const streakEl = el('streak'); if (streakEl) streakEl.textContent = streak;
    const pointsEl = el('points'); if (pointsEl) pointsEl.textContent = attemptPoints;

    el('feedback').className = 'feedback ' + (isCorrect ? 'ok' : 'bad');
    el('feedback').innerHTML = `<strong>${isCorrect ? 'Correcta' : 'Incorrecta'}.</strong> ${item.explanation}`;

    updateGlobalPanel();
    setTimeout(() => { current++; current < questions.length ? renderQuestion() : finishQuiz(); }, 850);
  }

  /* ── Finalizar intento ── */
  function finishQuiz() {
    quizFinished   = true;
    const total    = questions.length;
    const minScore = Math.ceil(total * CONFIG.passThreshold);
    const passed   = (score >= minScore) || (basicTotal > 0 && basicCorrect >= basicTotal);
    const stars    = score >= 9 ? 3 : score >= 7 ? 2 : score >= 5 ? 1 : 0;
    const existing = globalState.results[id] || defaultResult(total);
    const usedNow  = (existing.attempts || 0) + 1;
    const remaining = Math.max(0, CONFIG.maxAttempts - usedNow);

    el('bar').style.width      = '100%';
    el('progress').textContent = 'Prueba completada';
    el('options').innerHTML    = '';
    el('level').textContent    = passed ? 'Superado' : 'Pendiente';
    el('question').textContent = passed ? 'Bloque superado.' : 'Bloque completado, pero no desbloqueado.';

    el('feedback').className = 'feedback ' + (passed ? 'ok' : 'bad');
    el('feedback').innerHTML = `<strong>Puntuación:</strong> ${score}/${total}. Básicas correctas: ${basicCorrect}/${basicTotal}.`;

    el('reward').className    = 'reward ' + (passed ? 'ok' : 'bad');
    el('reward').style.display= 'block';
    el('reward').innerHTML    = passed
      ? `<strong>Desbloqueo conseguido.</strong> Has ganado ${stars} estrella${stars !== 1 ? 's' : ''} y puedes avanzar. Intentos restantes: ${remaining}.`
      : `<strong>Aún no desbloqueado.</strong> Necesitas 70 % o acertar todas las preguntas básicas. Intentos restantes: ${remaining}.`;

    const gate = el('gate');
    gate.className    = 'gate-note';
    gate.style.display= 'block';
    gate.innerHTML    = buildGateSummary(id, basicCorrect, basicTotal, score, total);

    el('review').style.display  = 'block';
    el('review').innerHTML      = buildReviewMessage(id, tagErrors);
    el('restart').style.display = remaining > 0 ? 'inline-flex' : 'none';

    /* Actualizar resultado en globalState */
    /* Guardamos el mejor basicCorrect de todos los intentos.
       basicTotal se actualiza junto al intento que produjo ese mejor resultado,
       para que la comparación basicCorrect >= basicTotal sea siempre coherente. */
    const prevBest      = existing.basicCorrect || 0;
    const newBestCorrect= Math.max(prevBest, basicCorrect);
    const newBestTotal  = newBestCorrect > prevBest ? basicTotal : (existing.basicTotal || basicTotal);

    globalState.results[id] = {
      ...existing,
      bestScore:    Math.max(existing.bestScore || 0, score),
      lastScore:    score,
      total,
      attempts:     usedNow,
      stars:        Math.max(existing.stars || 0, stars),
      basicCorrect: newBestCorrect,
      basicTotal:   newBestTotal,
      unlocked:     passed,
    };

    if (UNIT_IDS.includes(id) && passed) {
      globalState.completedUnits.add(id);
      unlockBadge(id, score);
    }

    globalState.stars = ALL_IDS.reduce((acc, key) => acc + (globalState.results[key]?.stars || 0), 0);

    saveProfile();
    updateGlobalPanel();
    refreshAll();
  }

  /* ── Reiniciar ── */
  el('restart').addEventListener('click', () => {
    prepare();
    el('score').textContent     = '0';
    const se = el('streak'); if (se) se.textContent = '0';
    const pe = el('points'); if (pe) pe.textContent = '0';
    el('restart').style.display = 'none';
    renderQuestion();
    saveProfile();
  });

  prepare();
  renderQuestion();
  /* refresh: llamado por refreshAll() tras cambios externos (desbloqueo, carga de perfil).
     - Si el bloque sigue bloqueado → muestra estado bloqueado.
     - Si el bloque está terminado (quizFinished) → no toca la pantalla de resultados.
     - Si el bloque acaba de desbloquearse y no ha empezado → prepara y muestra primera pregunta. */
  function refresh() {
    if (!isQuizUnlocked(id)) { renderLocked();     return; }
    if (!attemptsLeft(id))   { renderNoAttempts(); return; }
    if (quizFinished)        { return; }          /* resultados ya visibles, no tocar */
    if (current === 0)       { prepare(); renderQuestion(); } /* recién desbloqueado */
  }

  return { refresh };
}

/* ═══════════════════════════════════════════════════════════════════
   ARRANQUE
═══════════════════════════════════════════════════════════════════ */
renderQuizSections();
hydrateResults();

const quizzes = Object.fromEntries(ALL_IDS.map(id => [id, createQuiz(id)]));

function refreshAll() { Object.values(quizzes).forEach(q => q.refresh()); }
window.refreshAll = refreshAll;

updateGlobalPanel();
</script>
</body>
</html>
