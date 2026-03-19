<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Compatibility Profile Survey</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,600;1,400&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<style>
  :root {
    --bg: #0f0e11;
    --surface: #1a1820;
    --surface2: #221f2a;
    --border: #2e2b38;
    --accent: #c8a97e;
    --accent2: #8b6f9e;
    --text: #e8e4f0;
    --muted: #7a7585;
    --success: #6db98a;
    --radius: 12px;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'DM Sans', sans-serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* Subtle grain overlay */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 0;
    opacity: 0.4;
  }

  .app { position: relative; z-index: 1; max-width: 760px; margin: 0 auto; padding: 40px 24px 80px; }

  /* ── Header ── */
  .header { text-align: center; padding: 60px 0 48px; }
  .header-eyebrow {
    font-family: 'DM Sans', sans-serif;
    font-size: 11px;
    font-weight: 500;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--accent);
    margin-bottom: 16px;
  }
  .header h1 {
    font-family: 'Playfair Display', serif;
    font-size: clamp(32px, 6vw, 52px);
    line-height: 1.15;
    color: var(--text);
    margin-bottom: 16px;
  }
  .header h1 em { font-style: italic; color: var(--accent); }
  .header p {
    font-size: 15px;
    color: var(--muted);
    line-height: 1.7;
    max-width: 480px;
    margin: 0 auto 32px;
  }
  .partner-badge {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 100px;
    padding: 8px 18px;
    font-size: 13px;
    color: var(--muted);
  }
  .partner-badge .dot {
    width: 8px; height: 8px; border-radius: 50%;
    background: var(--accent2);
    animation: pulse 2s infinite;
  }
  @keyframes pulse { 0%,100%{opacity:1} 50%{opacity:0.4} }

  /* ── Progress ── */
  .progress-wrap { margin-bottom: 40px; }
  .progress-meta {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 10px;
    font-size: 12px;
    color: var(--muted);
  }
  .progress-bar {
    height: 3px;
    background: var(--border);
    border-radius: 2px;
    overflow: hidden;
  }
  .progress-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--accent2), var(--accent));
    border-radius: 2px;
    transition: width 0.5s cubic-bezier(0.4,0,0.2,1);
  }

  /* ── Section dividers ── */
  .layer-header {
    display: flex;
    align-items: center;
    gap: 14px;
    margin: 48px 0 24px;
  }
  .layer-pill {
    font-size: 10px;
    font-weight: 500;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    padding: 4px 12px;
    border-radius: 100px;
    white-space: nowrap;
  }
  .layer-1 .layer-pill { background: #1e2d3a; color: #7ab8d4; border: 1px solid #2a4a5e; }
  .layer-2 .layer-pill { background: #1e3328; color: #6db98a; border: 1px solid #2a5040; }
  .layer-3 .layer-pill { background: #3a2e1e; color: #c8a97e; border: 1px solid #5e4a2a; }
  .layer-4 .layer-pill { background: #2e1e3a; color: #a97ec8; border: 1px solid #4a2a5e; }
  .layer-line { flex: 1; height: 1px; background: var(--border); }
  .layer-title {
    font-family: 'Playfair Display', serif;
    font-size: 13px;
    color: var(--muted);
    white-space: nowrap;
  }

  /* ── Question card ── */
  .question-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 28px 28px 24px;
    margin-bottom: 16px;
    transition: border-color 0.2s;
  }
  .question-card:focus-within { border-color: var(--accent2); }
  .question-card.answered { border-color: rgba(109,185,138,0.3); }

  .q-label {
    font-size: 11px;
    font-weight: 500;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 10px;
  }
  .q-text {
    font-family: 'Playfair Display', serif;
    font-size: 18px;
    line-height: 1.5;
    color: var(--text);
    margin-bottom: 20px;
  }
  .q-sub {
    font-size: 13px;
    color: var(--muted);
    margin-top: -14px;
    margin-bottom: 18px;
    line-height: 1.5;
  }

  /* ── Input: text/number ── */
  input[type="text"], input[type="number"], input[type="email"] {
    width: 100%;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 8px;
    color: var(--text);
    font-family: 'DM Sans', sans-serif;
    font-size: 15px;
    padding: 12px 16px;
    outline: none;
    transition: border-color 0.2s;
  }
  input[type="text"]:focus, input[type="number"]:focus, input[type="email"]:focus {
    border-color: var(--accent2);
  }

  /* ── Input: radio/option tiles ── */
  .options { display: flex; flex-wrap: wrap; gap: 10px; }
  .options.vertical { flex-direction: column; }
  .option-tile {
    display: flex;
    align-items: center;
    gap: 10px;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 11px 16px;
    cursor: pointer;
    font-size: 14px;
    color: var(--muted);
    transition: all 0.15s;
    user-select: none;
  }
  .options:not(.vertical) .option-tile { flex: 0 0 auto; }
  .options.vertical .option-tile { flex: 1; }
  .option-tile:hover { border-color: var(--accent2); color: var(--text); }
  .option-tile input { display: none; }
  .option-tile.selected {
    background: rgba(139,111,158,0.15);
    border-color: var(--accent2);
    color: var(--text);
  }
  .option-tile .check {
    width: 18px; height: 18px;
    border-radius: 50%;
    border: 1.5px solid var(--border);
    display: flex; align-items: center; justify-content: center;
    flex-shrink: 0;
    transition: all 0.15s;
    font-size: 10px;
  }
  .option-tile.selected .check {
    background: var(--accent2);
    border-color: var(--accent2);
    color: white;
  }

  /* ── Slider ── */
  .slider-wrap { padding: 4px 0 8px; }
  .slider-labels {
    display: flex;
    justify-content: space-between;
    font-size: 12px;
    color: var(--muted);
    margin-bottom: 10px;
  }
  input[type="range"] {
    -webkit-appearance: none;
    width: 100%;
    height: 4px;
    background: var(--border);
    border-radius: 2px;
    outline: none;
  }
  input[type="range"]::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 20px; height: 20px;
    border-radius: 50%;
    background: var(--accent);
    cursor: pointer;
    border: 2px solid var(--bg);
    box-shadow: 0 0 0 2px var(--accent);
  }
  .slider-value {
    text-align: center;
    font-size: 13px;
    color: var(--accent);
    margin-top: 8px;
    font-weight: 500;
  }

  /* ── Multi-select tags ── */
  .tags { display: flex; flex-wrap: wrap; gap: 8px; }
  .tag {
    display: inline-flex; align-items: center; gap: 6px;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 100px;
    padding: 7px 14px;
    font-size: 13px;
    color: var(--muted);
    cursor: pointer;
    transition: all 0.15s;
    user-select: none;
  }
  .tag:hover { border-color: var(--accent); color: var(--text); }
  .tag.selected {
    background: rgba(200,169,126,0.15);
    border-color: var(--accent);
    color: var(--accent);
  }
  .tag-note { font-size: 12px; color: var(--muted); margin-top: 10px; }

  /* ── Interest grid ── */
  .interest-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
  @media(max-width: 500px) { .interest-grid { grid-template-columns: 1fr; } }
  .interest-item {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 12px 14px;
    cursor: pointer;
    transition: border-color 0.15s;
  }
  .interest-item:hover { border-color: var(--border); }
  .interest-name { font-size: 14px; color: var(--text); margin-bottom: 8px; }
  .importance-btns { display: flex; gap: 6px; }
  .imp-btn {
    flex: 1; font-size: 11px; padding: 4px 6px;
    background: var(--surface); border: 1px solid var(--border);
    border-radius: 6px; color: var(--muted); cursor: pointer;
    text-align: center; transition: all 0.15s; font-family: 'DM Sans', sans-serif;
  }
  .imp-btn:hover { color: var(--text); }
  .imp-btn.active-core { background: rgba(200,169,126,0.2); border-color: var(--accent); color: var(--accent); }
  .imp-btn.active-enjoy { background: rgba(139,111,158,0.2); border-color: var(--accent2); color: var(--accent2); }
  .imp-btn.active-nice { background: rgba(109,185,138,0.1); border-color: var(--success); color: var(--success); }

  /* ── Navigation ── */
  .nav-wrap {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 40px;
    gap: 16px;
  }
  .btn {
    font-family: 'DM Sans', sans-serif;
    font-size: 14px;
    font-weight: 500;
    padding: 13px 28px;
    border-radius: 8px;
    border: none;
    cursor: pointer;
    transition: all 0.2s;
    letter-spacing: 0.02em;
  }
  .btn-primary {
    background: var(--accent);
    color: #0f0e11;
    flex: 1;
    max-width: 280px;
  }
  .btn-primary:hover { background: #d4b88e; transform: translateY(-1px); }
  .btn-secondary {
    background: transparent;
    color: var(--muted);
    border: 1px solid var(--border);
  }
  .btn-secondary:hover { color: var(--text); border-color: var(--muted); }
  .btn:disabled { opacity: 0.4; cursor: not-allowed; transform: none; }

  /* ── Results ── */
  #results-view { display: none; }
  .result-hero {
    text-align: center;
    padding: 48px 0 40px;
  }
  .score-ring {
    width: 160px; height: 160px;
    margin: 0 auto 24px;
    position: relative;
  }
  .score-ring svg { transform: rotate(-90deg); }
  .score-ring .score-text {
    position: absolute; inset: 0;
    display: flex; flex-direction: column;
    align-items: center; justify-content: center;
    transform: none;
  }
  .score-number {
    font-family: 'Playfair Display', serif;
    font-size: 40px;
    color: var(--accent);
    line-height: 1;
  }
  .score-label { font-size: 11px; color: var(--muted); letter-spacing: 0.1em; text-transform: uppercase; }
  .result-title {
    font-family: 'Playfair Display', serif;
    font-size: 28px;
    margin-bottom: 8px;
  }
  .result-subtitle { font-size: 15px; color: var(--muted); }

  .breakdown-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
    margin: 32px 0;
  }
  @media(max-width: 500px) { .breakdown-grid { grid-template-columns: 1fr; } }
  .breakdown-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 20px;
  }
  .bc-label { font-size: 11px; color: var(--muted); letter-spacing: 0.1em; text-transform: uppercase; margin-bottom: 8px; }
  .bc-score {
    font-family: 'Playfair Display', serif;
    font-size: 28px;
    color: var(--text);
    margin-bottom: 4px;
  }
  .bc-bar { height: 3px; background: var(--border); border-radius: 2px; overflow: hidden; }
  .bc-fill { height: 100%; border-radius: 2px; }
  .bc-note { font-size: 12px; color: var(--muted); margin-top: 8px; line-height: 1.5; }

  .insight-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 20px 24px;
    margin-bottom: 12px;
  }
  .insight-type {
    font-size: 10px; letter-spacing: 0.15em; text-transform: uppercase;
    font-weight: 500; margin-bottom: 6px;
  }
  .insight-type.strength { color: var(--success); }
  .insight-type.tension { color: #d4846a; }
  .insight-text { font-size: 15px; line-height: 1.6; color: var(--text); }

  .result-cta {
    text-align: center;
    margin-top: 32px;
    padding-top: 32px;
    border-top: 1px solid var(--border);
  }
  .result-cta p { font-size: 13px; color: var(--muted); margin-bottom: 16px; }

  /* ── Page screens ── */
  #survey-view, #intro-view { }
  #intro-view { text-align: center; padding: 80px 0; }
  .intro-cards {
    display: grid; grid-template-columns: 1fr 1fr; gap: 12px;
    margin: 40px 0;
    text-align: left;
  }
  @media(max-width:480px){.intro-cards{grid-template-columns:1fr;}}
  .intro-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 20px;
  }
  .intro-card-icon { font-size: 22px; margin-bottom: 10px; }
  .intro-card h3 { font-size: 14px; font-weight: 500; color: var(--text); margin-bottom: 6px; }
  .intro-card p { font-size: 13px; color: var(--muted); line-height: 1.5; }

  .name-inputs { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin: 24px 0 32px; }
  @media(max-width:480px){.name-inputs{grid-template-columns:1fr;}}
  .name-input-wrap label { display: block; font-size: 12px; color: var(--muted); letter-spacing: 0.1em; text-transform: uppercase; margin-bottom: 8px; }

  /* Fade animation */
  .fade-in { animation: fadeIn 0.4s ease forwards; }
  @keyframes fadeIn { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: translateY(0); } }
</style>
</head>
<body>
<div class="app">

  <!-- ── INTRO ── -->
  <div id="intro-view">
    <div class="header-eyebrow">Compatibility Research</div>
    <h1 style="font-family:'Playfair Display',serif;font-size:clamp(28px,5vw,44px);line-height:1.2;margin-bottom:16px;">
      What makes two people <em style="color:var(--accent);font-style:italic;">truly</em> compatible?
    </h1>
    <p style="font-size:15px;color:var(--muted);line-height:1.7;max-width:460px;margin:0 auto 8px;">
      A research-backed profile survey for couples. Each partner fills it out independently. We'll show you where you align — and where the interesting differences live.
    </p>
    <div class="intro-cards">
      <div class="intro-card"><div class="intro-card-icon">🔒</div><h3>Private</h3><p>Answers stay in your browser. Nothing is sent anywhere.</p></div>
      <div class="intro-card"><div class="intro-card-icon">⏱</div><h3>~8 minutes</h3><p>32 questions across 4 layers of compatibility.</p></div>
      <div class="intro-card"><div class="intro-card-icon">📊</div><h3>Research-backed</h3><p>Based on Gottman, similarity-attraction, and complementarity research.</p></div>
      <div class="intro-card"><div class="intro-card-icon">💬</div><h3>Conversation starter</h3><p>Results designed to spark honest discussion, not judge your relationship.</p></div>
    </div>
    <p style="font-size:14px;color:var(--muted);margin-bottom:16px;">Enter both partners' names to begin:</p>
    <div class="name-inputs">
      <div class="name-input-wrap">
        <label>Partner 1</label>
        <input type="text" id="name1" placeholder="e.g. Alex" />
      </div>
      <div class="name-input-wrap">
        <label>Partner 2</label>
        <input type="text" id="name2" placeholder="e.g. Jordan" />
      </div>
    </div>
    <button class="btn btn-primary" onclick="startSurvey()" style="font-size:15px;padding:14px 40px;">Begin Survey →</button>
  </div>

  <!-- ── SURVEY ── -->
  <div id="survey-view" style="display:none;">
    <div class="progress-wrap">
      <div class="progress-meta">
        <span id="progress-label">Question 1 of 30</span>
        <span id="progress-pct">0%</span>
      </div>
      <div class="progress-bar"><div class="progress-fill" id="progress-fill" style="width:0%"></div></div>
    </div>
    <div id="questions-container"></div>
    <div class="nav-wrap">
      <button class="btn btn-secondary" id="prev-btn" onclick="navigate(-1)">← Back</button>
      <button class="btn btn-primary" id="next-btn" onclick="navigate(1)">Continue →</button>
    </div>
  </div>

  <!-- ── RESULTS ── -->
  <div id="results-view"></div>

</div>

<script>
let p1Name = 'Partner 1', p2Name = 'Partner 2';
let currentPartner = 1;
let answers = { p1: {}, p2: {} };
let currentQ = 0;
const DEBUG = new URLSearchParams(window.location.search).has('debug');

const SLIDER_LABELS = {
  careerVsRelationships: ['All career', 'Balanced', 'All relationships'],
  moneyStyle: ['Save first always', 'Balanced', 'Live well now'],
  introExtro: ['Deep introvert', 'Ambivert', 'Strong extrovert'],
  decisionStyle: ['Gut feeling fast', 'Balanced', 'Research deeply'],
  activityLevel: ['Sedentary', 'Moderately active', 'Very active'],
  tidiness: ['Very relaxed', 'Moderate', 'Highly organized'],
  politicalAlign: ['Far left', 'Center', 'Far right'],
  religiosity: ['Not part of my daily life', 'Somewhat present', 'Central to how I live'],
};

const questions = [
  // LAYER 1
  { id:'age', layer:1, text:'How old are you?', type:'number', placeholder:'Enter your age' },
  { id:'gender', layer:1, text:'How do you identify?', type:'radio', options:['Man','Woman','Non-binary','Genderqueer','Prefer to self-describe'] },
  { id:'bodyType', layer:1, text:'How would you describe your body type?', type:'radio', options:['Slim','Athletic','Average','Curvy / Stocky','Plus-size'] },
  { id:'orientation', layer:1, text:'What best describes your sexual orientation?', type:'radio', options:['Straight','Gay / Lesbian','Bisexual','Pansexual','Prefer to self-describe'] },
  { id:'relationshipGoal', layer:1, text:'What are you looking for in a relationship?', type:'radio', options:['Casual connection','Long-term partnership','Open to both','Not sure yet'] },
  { id:'wantsKids', layer:1, text:'How do you feel about having children?', type:'radio', options:['Yes, I want kids','No, I don\'t want kids','I have kids and I\'m done','Open to it'] },
  { id:'smoking', layer:1, text:'Do you smoke?', type:'radio', options:['Never','Socially only','Regularly'] },
  { id:'drinking', layer:1, text:'Do you drink alcohol?', type:'radio', options:['Never','Socially only','Regularly'] },
  { id:'religion', layer:1, text:'What is your religion or spiritual identity?', type:'radio', options:['Christian','Muslim','Jewish','Hindu','Buddhist','Spiritual but not religious','Agnostic','Atheist','Other'] },
  { id:'religiosity', layer:1, text:'How central is your faith or spiritual practice to your daily life?', type:'slider', key:'religiosity' },
  { id:'politics', layer:1, text:'Where do you sit politically?', sub:'Used only to assess alignment, not to judge.', type:'slider', key:'politicalAlign' },

  // LAYER 2
  { id:'careerVsRelationships', layer:2, text:'Right now in life, which matters more to you?', type:'slider', key:'careerVsRelationships' },
  { id:'tenYearGoals', layer:2, text:'In 10 years, what feels most important to you?', sub:'Pick up to 2.', type:'multiselect', options:['Financial security','Adventure & exploration','Starting / growing a family','Creative fulfillment','Community & belonging','Personal growth'], max:2 },
  { id:'moneyStyle', layer:2, text:'How do you think about money?', type:'slider', key:'moneyStyle' },
  { id:'ambition', layer:2, text:'What\'s your relationship with personal ambition?', type:'radio', options:['Very driven — I push hard toward goals','I work to live, not live to work','Somewhere comfortably in between'] },
  { id:'collectivism', layer:2, text:'How important is social justice and collective responsibility to you?', type:'radio', options:['Core to my identity — it shapes my choices','Important, but not the center of my life','I focus more on personal responsibility'] },
  { id:'familyRole', layer:2, text:'What role does your family (parents, siblings) play in your life?', type:'radio', options:['Central — we\'re very involved in each other\'s lives','Close but independent','I\'ve built my own chosen family','It\'s complicated'] },

  // LAYER 3
  { id:'introExtro', layer:3, text:'In social situations, you tend to...', type:'slider', key:'introExtro' },
  { id:'decisionStyle', layer:3, text:'When making decisions, you tend to...', type:'slider', key:'decisionStyle' },
  { id:'dominance', layer:3, text:'In a partnership, you naturally see yourself as...', type:'radio', options:['The planner and organizer','The one who goes with the flow','Equal co-pilots — it depends'] },
  { id:'conflictDominance', layer:3, text:'When a conflict arises, you usually...', type:'radio', options:['Take charge and push for resolution','Step back and let things settle','Adapt to whatever the other person needs'] },
  { id:'loveLanguageGive', layer:3, text:'How do you most naturally show care?', sub:'Pick your top 2.', type:'multiselect', options:['Quality time','Words of affirmation','Acts of service','Physical affection','Thoughtful gifts'], max:2 },
  { id:'loveLanguageReceive', layer:3, text:'How do you most feel loved and cared for?', sub:'Pick your top 2.', type:'multiselect', options:['Quality time','Words of affirmation','Acts of service','Physical affection','Thoughtful gifts'], max:2 },
  { id:'stressNeed', layer:3, text:'When you\'re stressed, what do you want from a partner?', type:'radio', options:['Help me solve the problem','Just listen and be present','Give me space to process'] },
  { id:'stressOffer', layer:3, text:'What do you naturally offer when your partner is stressed?', type:'radio', options:['Jump in to help solve it','Listen without trying to fix it','Give them space'] },
  { id:'conflictStyle', layer:3, text:'How do you handle conflict in relationships?', type:'radio', options:['Address it immediately — I can\'t let it sit','I need time to process before talking','I tend to let small things go'] },

  // LAYER 4
  { id:'activityLevel', layer:4, text:'How physically active are you day-to-day?', type:'slider', key:'activityLevel' },
  { id:'diet', layer:4, text:'How would you describe your diet?', type:'radio', options:['Omnivore','Flexitarian','Vegetarian','Vegan','No real preference'] },
  { id:'schedule', layer:4, text:'Are you more of a night owl or an early riser?', type:'radio', options:['Definitely a night owl','Slight night owl','Somewhere in between','Slight early riser','Definitely an early riser'] },
  { id:'tidiness', layer:4, text:'How tidy is your living space?', type:'slider', key:'tidiness' },
  { id:'livingPref', layer:4, text:'Where do you most want to live?', type:'radio', options:['City center','Suburbs','Small town','Rural / nature','Flexible'] },
  { id:'interests', layer:4, text:'Select your interests and how central each is to your life.', sub:'Only select what\'s genuinely true. "Now & then" means occasional — something you do about once a month. "Core to me" means you\'d genuinely miss it.', type:'interests',
    options:['Outdoors & hiking','Arts & culture','Food & cooking','Sports','Music','Travel','Fitness / gym','Gaming','Reading','Building & making things','Social causes','Film & TV','Dance','Spirituality'] },
];

const TOTAL = questions.length;

function getLayerInfo(layer) {
  const map = {
    1: { label: 'Layer 1', title: 'Hard Filters', cls: 'layer-1' },
    2: { label: 'Layer 2', title: 'Values & Worldview', cls: 'layer-2' },
    3: { label: 'Layer 3', title: 'Personality & Dynamics', cls: 'layer-3' },
    4: { label: 'Layer 4', title: 'Lifestyle & Interests', cls: 'layer-4' },
  };
  return map[layer];
}

function startSurvey() {
  const n1 = document.getElementById('name1').value.trim();
  const n2 = document.getElementById('name2').value.trim();
  if (!n1 || !n2) { alert('Please enter both names to continue.'); return; }
  p1Name = n1; p2Name = n2;
  document.getElementById('intro-view').style.display = 'none';
  document.getElementById('survey-view').style.display = 'block';
  currentPartner = 1; currentQ = 0;
  renderQuestion();
}

function getAnswers() { return currentPartner === 1 ? answers.p1 : answers.p2; }
function getCurrentName() { return currentPartner === 1 ? p1Name : p2Name; }

function renderQuestion() {
  const q = questions[currentQ];
  const li = getLayerInfo(q.layer);
  const ans = getAnswers();
  const container = document.getElementById('questions-container');

  // Show layer header if first question of a new layer
  const prevLayer = currentQ > 0 ? questions[currentQ - 1].layer : null;
  let html = '';
  if (q.layer !== prevLayer) {
    html += `<div class="layer-header ${li.cls}">
      <span class="layer-pill">${li.label}</span>
      <div class="layer-line"></div>
      <span class="layer-title">${li.title}</span>
    </div>`;
  }

  html += `<div class="question-card fade-in ${ans[q.id] !== undefined ? 'answered' : ''}" id="qcard">
    <div class="q-label">${getCurrentName()} · Question ${currentQ + 1} of ${TOTAL}</div>
    <div class="q-text">${q.text}</div>
    ${q.sub ? `<div class="q-sub">${q.sub}</div>` : ''}
    ${renderInput(q, ans[q.id])}
  </div>`;

  container.innerHTML = html;
  updateProgress();
  document.getElementById('prev-btn').disabled = currentQ === 0 && currentPartner === 1;
  document.getElementById('next-btn').textContent = isLastQuestion() ? 'See Results →' : 'Continue →';

  // Attach events
  attachInputEvents(q, ans);
  window.scrollTo({ top: 0, behavior: 'smooth' });
  renderDebugPanel();
}

function renderInput(q, currentVal) {
  if (q.type === 'number') {
    return `<input type="number" id="qinput" value="${currentVal || ''}" placeholder="${q.placeholder}" min="16" max="99" style="max-width:200px;" oninput="saveAnswer('${q.id}', this.value)">`;
  }
  if (q.type === 'radio') {
    return `<div class="options vertical">${q.options.map(o =>
      `<label class="option-tile ${currentVal === o ? 'selected' : ''}">
        <input type="radio" name="q${q.id}" value="${o}" ${currentVal === o ? 'checked' : ''}>
        <span class="check">${currentVal === o ? '✓' : ''}</span>
        <span>${o}</span>
      </label>`
    ).join('')}</div>`;
  }
  if (q.type === 'slider') {
    const lbls = SLIDER_LABELS[q.key] || ['Low', 'Mid', 'High'];
    const val = currentVal !== undefined ? currentVal : 2;
    return `<div class="slider-wrap">
      <div class="slider-labels"><span>${lbls[0]}</span><span>${lbls[2]}</span></div>
      <input type="range" id="qinput" min="0" max="4" value="${val}" oninput="saveAnswer('${q.id}', parseInt(this.value)); updateSliderDisplay(this, '${q.key}')">
      <div class="slider-value" id="slider-display">${getSliderLabel(val, q.key)}</div>
    </div>`;
  }
  if (q.type === 'multiselect') {
    const sel = currentVal || [];
    return `<div class="tags">${q.options.map(o =>
      `<span class="tag ${sel.includes(o) ? 'selected' : ''}" onclick="toggleTag('${q.id}', '${o}', ${q.max || 99}, this)">${o}</span>`
    ).join('')}</div>
    <div class="tag-note">Select up to ${q.max}.</div>`;
  }
  if (q.type === 'interests') {
    const state = currentVal || {};
    return `<div class="interest-grid">${q.options.map(o => {
      const s = state[o] || 0;
      return `<div class="interest-item">
        <div class="interest-name">${o}</div>
        <div class="importance-btns">
          <button class="imp-btn ${s===1?'active-enjoy':''}" onclick="setInterest('${q.id}', '${o}', 1, this.parentElement)">Now & then</button>
          <button class="imp-btn ${s===2?'active-core':''}" onclick="setInterest('${q.id}', '${o}', 2, this.parentElement)">Core to me</button>
        </div>
      </div>`;
    }).join('')}</div>`;
  }
  return '';
}

function getSliderLabel(val, key) {
  const lbls = SLIDER_LABELS[key] || ['Low','Mid','High'];
  const labels5 = [lbls[0], `Leans ${lbls[0].split(' ')[0].toLowerCase()}`, lbls[1], `Leans ${lbls[2].split(' ')[0].toLowerCase()}`, lbls[2]];
  return labels5[val] || lbls[1];
}

function updateSliderDisplay(el, key) {
  document.getElementById('slider-display').textContent = getSliderLabel(parseInt(el.value), key);
}

function attachInputEvents(q, ans) {
  if (q.type === 'radio') {
    document.querySelectorAll('.option-tile').forEach(tile => {
      tile.addEventListener('click', function() {
        const val = this.querySelector('input').value;
        document.querySelectorAll('.option-tile').forEach(t => {
          t.classList.remove('selected');
          t.querySelector('.check').textContent = '';
        });
        this.classList.add('selected');
        this.querySelector('.check').textContent = '✓';
        saveAnswer(q.id, val);
      });
    });
  }
}

function saveAnswer(id, val) {
  if (currentPartner === 1) answers.p1[id] = val;
  else answers.p2[id] = val;
  const card = document.getElementById('qcard');
  if (card) card.classList.add('answered');
}

function toggleTag(id, val, max, el) {
  const ans = getAnswers();
  let sel = ans[id] ? [...ans[id]] : [];
  if (sel.includes(val)) { sel = sel.filter(s => s !== val); el.classList.remove('selected'); }
  else if (sel.length < max) { sel.push(val); el.classList.add('selected'); }
  saveAnswer(id, sel);
}

function setInterest(id, interest, level, btnsEl) {
  const ans = getAnswers();
  const state = ans[id] ? { ...ans[id] } : {};
  if (state[interest] === level) { state[interest] = 0; }
  else { state[interest] = level; }
  saveAnswer(id, state);
  const btns = btnsEl.querySelectorAll('.imp-btn');
  btns[0].className = `imp-btn ${state[interest]===1?'active-enjoy':''}`;
  btns[1].className = `imp-btn ${state[interest]===2?'active-core':''}`;
}

function updateProgress() {
  const total = TOTAL * 2;
  const done = Object.keys(answers.p1).length + Object.keys(answers.p2).length;
  const pct = Math.round((done / total) * 100);
  const partnerOffset = currentPartner === 2 ? TOTAL : 0;
  const qNum = partnerOffset + currentQ + 1;
  document.getElementById('progress-label').textContent = `${getCurrentName()} · Q${currentQ + 1} of ${TOTAL}`;
  document.getElementById('progress-pct').textContent = pct + '%';
  document.getElementById('progress-fill').style.width = pct + '%';
}

function isLastQuestion() {
  return currentPartner === 2 && currentQ === TOTAL - 1;
}

function navigate(dir) {
  if (dir === 1 && isLastQuestion()) { showResults(); return; }
  if (dir === 1) {
    if (currentQ < TOTAL - 1) { currentQ++; }
    else if (currentPartner === 1) {
      currentPartner = 2; currentQ = 0;
      // Brief transition message
      showPartnerTransition();
      return;
    }
  } else {
    if (currentQ > 0) { currentQ--; }
    else if (currentPartner === 2) { currentPartner = 1; currentQ = TOTAL - 1; }
  }
  renderQuestion();
}

function showPartnerTransition() {
  const container = document.getElementById('questions-container');
  container.innerHTML = `<div class="question-card fade-in" style="text-align:center;padding:48px 28px;">
    <div style="font-size:40px;margin-bottom:16px;">🤝</div>
    <div style="font-family:'Playfair Display',serif;font-size:22px;margin-bottom:12px;">${p1Name} is done!</div>
    <p style="color:var(--muted);font-size:15px;line-height:1.6;max-width:380px;margin:0 auto 24px;">
      Now pass the device to <strong style="color:var(--text)">${p2Name}</strong> — they should answer without looking at ${p1Name}'s responses.
    </p>
    <button class="btn btn-primary" onclick="continueP2()" style="font-size:15px;padding:14px 36px;">I'm ready, ${p2Name} →</button>
  </div>`;
  document.getElementById('prev-btn').style.display = 'none';
  document.getElementById('next-btn').style.display = 'none';
}

function continueP2() {
  document.getElementById('prev-btn').style.display = '';
  document.getElementById('next-btn').style.display = '';
  renderQuestion();
}

// ══ SCORING ══════════════════════════════════════════════════════════════════

function scoreSliderSimilarity(v1, v2) {
  if (v1 === undefined || v2 === undefined) return 0.5;
  return 1 - Math.abs(v1 - v2) / 4;
}

function scoreRadioSimilarity(v1, v2) {
  if (!v1 || !v2) return 0.5;
  return v1 === v2 ? 1 : 0.2;
}

function scoreMultiselectOverlap(v1, v2) {
  if (!v1 || !v2 || !v1.length || !v2.length) return 0.5;
  const overlap = v1.filter(x => v2.includes(x)).length;
  return overlap / Math.max(v1.length, v2.length);
}

function scoreLoveLanguage(give1, recv2, give2, recv1) {
  // How well does each person's giving style match the other's receiving preference?
  const score1 = give1 && recv2 ? give1.filter(g => recv2.includes(g)).length / Math.max(give1.length||1, recv2.length||1) : 0.5;
  const score2 = give2 && recv1 ? give2.filter(g => recv1.includes(g)).length / Math.max(give2.length||1, recv1.length||1) : 0.5;
  return (score1 + score2) / 2;
}

function scoreComplementarity(dom1, dom2) {
  // Dominant/submissive pairing scores higher
  const map = { 'The planner and organizer': 2, 'Equal co-pilots — it depends': 1, 'The one who goes with the flow': 0 };
  const d1 = map[dom1] !== undefined ? map[dom1] : 1;
  const d2 = map[dom2] !== undefined ? map[dom2] : 1;
  const diff = Math.abs(d1 - d2);
  return diff === 2 ? 1 : diff === 1 ? 0.7 : 0.4;
}

function scoreStressMatch(need1, offer2, need2, offer1) {
  const match1 = need1 && offer2 ? scoreStressPair(need1, offer2) : 0.5;
  const match2 = need2 && offer1 ? scoreStressPair(need2, offer1) : 0.5;
  return (match1 + match2) / 2;
}

function scoreStressPair(need, offer) {
  const needMap = {
    'Help me solve the problem': 'Jump in to help solve it',
    'Just listen and be present': 'Listen without trying to fix it',
    'Give me space to process': 'Give them space'
  };
  return needMap[need] === offer ? 1 : 0.2;
}

function scoreInterests(int1, int2) {
  if (!int1 && !int2) return 0.5;
  const i1 = int1 || {}, i2 = int2 || {};
  const allKeys = new Set([...Object.keys(i1), ...Object.keys(i2)]);
  const coreItems = [...allKeys].filter(k => i1[k] === 2 || i2[k] === 2);
  if (!coreItems.length) return 0.5;
  const scores = coreItems.map(k => {
    const v1 = i1[k] || 0, v2 = i2[k] || 0;
    if (v1 === 2 && v2 === 2) return 1.0;
    if ((v1 === 2 && v2 === 1) || (v1 === 1 && v2 === 2)) return 0.5;
    return 0.15;
  });
  return scores.reduce((a, b) => a + b, 0) / scores.length;
}

function computeScores() {
  const p1 = answers.p1, p2 = answers.p2;

  // VALUES (40%)
  const v_career    = scoreSliderSimilarity(p1.careerVsRelationships, p2.careerVsRelationships);
  const v_goals     = scoreMultiselectOverlap(p1.tenYearGoals, p2.tenYearGoals);
  const v_money     = scoreSliderSimilarity(p1.moneyStyle, p2.moneyStyle);
  const v_ambition  = scoreRadioSimilarity(p1.ambition, p2.ambition);
  const v_collective  = scoreRadioSimilarity(p1.collectivism, p2.collectivism);
  const v_family      = scoreRadioSimilarity(p1.familyRole, p2.familyRole);
  const v_religiosity = scoreSliderSimilarity(p1.religiosity, p2.religiosity);
  const valuesScore = (v_career + v_goals + v_money + v_ambition + v_collective + v_family + v_religiosity) / 7;

  // COMPLEMENTARITY (20%)
  const c_dom = scoreComplementarity(p1.dominance, p2.dominance);
  const c_conflict = scoreComplementarity(p1.conflictDominance, p2.conflictDominance);
  const compScore = (c_dom + c_conflict) / 2;

  // DYNAMICS (25%)
  const d_ll    = scoreLoveLanguage(p1.loveLanguageGive, p2.loveLanguageReceive, p2.loveLanguageGive, p1.loveLanguageReceive);
  const d_stress= scoreStressMatch(p1.stressNeed, p2.stressOffer, p2.stressNeed, p1.stressOffer);
  const d_conflict = scoreRadioSimilarity(p1.conflictStyle, p2.conflictStyle);
  const d_intro = scoreSliderSimilarity(p1.introExtro, p2.introExtro);
  const dynamicsScore = (d_ll + d_stress + d_conflict + d_intro) / 4;

  // LIFESTYLE (10%)
  const l_activity = scoreSliderSimilarity(p1.activityLevel, p2.activityLevel);
  const l_diet     = scoreRadioSimilarity(p1.diet, p2.diet);
  const l_tidy     = scoreSliderSimilarity(p1.tidiness, p2.tidiness);
  const l_schedule = scoreRadioSimilarity(p1.schedule, p2.schedule);
  const l_location = scoreRadioSimilarity(p1.livingPref, p2.livingPref);
  const lifestyleScore = (l_activity + l_diet + l_tidy + l_schedule + l_location) / 5;

  // INTERESTS (5%)
  const interestsScore = scoreInterests(p1.interests, p2.interests);

  // TOTAL
  const total = valuesScore * 0.40 + compScore * 0.20 + dynamicsScore * 0.25 + lifestyleScore * 0.10 + interestsScore * 0.05;

  return {
    total: Math.round(total * 100),
    values: Math.round(valuesScore * 100),
    complementarity: Math.round(compScore * 100),
    dynamics: Math.round(dynamicsScore * 100),
    lifestyle: Math.round(lifestyleScore * 100),
    interests: Math.round(interestsScore * 100),
    detail: { v_career, v_goals, v_money, v_ambition, v_collective, v_family, d_ll, d_stress, d_conflict, l_tidy, l_location }
  };
}

function getInsights(scores) {
  const p1 = answers.p1, p2 = answers.p2;
  const insights = [];

  // Strengths
  if (scores.detail.v_goals > 0.7) insights.push({ type:'strength', text:`You share a strong vision for the future — both of you prioritize similar life goals over the next decade.` });
  if (scores.detail.d_ll > 0.7) insights.push({ type:'strength', text:`Your love languages are well-matched. The way each of you gives care closely aligns with how the other receives it.` });
  if (scores.detail.d_stress > 0.7) insights.push({ type:'strength', text:`When things get hard, you're naturally in sync. What ${p1Name} needs under stress is what ${p2Name} naturally offers, and vice versa.` });
  if (scores.complementarity > 65) insights.push({ type:'strength', text:`Your dynamic styles complement each other well — one of you tends to lead while the other flows, creating natural balance.` });
  if (scores.detail.v_money > 0.8) insights.push({ type:'strength', text:`You have closely aligned financial values — a surprisingly strong predictor of long-term relationship health.` });

  // Tensions
  if (scores.detail.v_money < 0.4) insights.push({ type:'tension', text:`Your approaches to money diverge noticeably. Research flags this as one of the top sources of recurring conflict. Worth an honest conversation.` });
  if (scores.detail.d_ll < 0.4) insights.push({ type:'tension', text:`There's a mismatch in love languages — the way each of you gives affection may not land the way the other needs to receive it.` });
  if (scores.detail.d_stress < 0.4) insights.push({ type:'tension', text:`Under stress, you each need something different from a partner — and what you naturally offer may not match what they need.` });
  if (scores.detail.v_goals < 0.4) insights.push({ type:'tension', text:`Your 10-year visions are pointing in different directions. This doesn't mean incompatibility, but it's worth exploring what you each actually want.` });
  if (scores.detail.l_tidy < 0.3) insights.push({ type:'tension', text:`Your tidiness styles are on opposite ends. Day-to-day living habits are a surprisingly common friction source — small thing, but real.` });

  // Cap to 3 of each
  const strengths = insights.filter(i => i.type === 'strength').slice(0, 3);
  const tensions = insights.filter(i => i.type === 'tension').slice(0, 3);
  return [...strengths, ...tensions];
}

async function shareResults() {
  const btn = document.getElementById('share-btn');
  btn.textContent = 'Capturing…';
  btn.disabled = true;

  try {
    const el = document.getElementById('results-view');
    const canvas = await html2canvas(el, { backgroundColor: '#0f0e11', scale: 2, useCORS: true });

    canvas.toBlob(async (blob) => {
      const file = new File([blob], 'compatibility-results.png', { type: 'image/png' });
      if (navigator.canShare && navigator.canShare({ files: [file] })) {
        await navigator.share({ files: [file], title: 'Compatibility Results' });
      } else {
        // Fallback: download the image
        const url = URL.createObjectURL(blob);
        const a = document.createElement('a');
        a.href = url; a.download = 'compatibility-results.png'; a.click();
        URL.revokeObjectURL(url);
      }
      btn.textContent = 'Share Results ↗';
      btn.disabled = false;
    }, 'image/png');
  } catch(e) {
    btn.textContent = 'Share Results ↗';
    btn.disabled = false;
  }
}

function getTitle(score) {
  if (score >= 80) return 'Deeply Aligned';
  if (score >= 65) return 'Strongly Compatible';
  if (score >= 50) return 'Good Foundation';
  if (score >= 35) return 'Complementary Differences';
  return 'Growth Edges Ahead';
}

function showResults() {
  document.getElementById('survey-view').style.display = 'none';
  const scores = computeScores();
  const insights = getInsights(scores);
  const title = getTitle(scores.total);

  const circumference = 2 * Math.PI * 60;
  const filled = (scores.total / 100) * circumference;

  const breakdowns = [
    { label: 'Values & Worldview', score: scores.values, weight: '40%', color: '#6db98a', note: 'Strongest long-term predictor of satisfaction.' },
    { label: 'Dynamics & Communication', score: scores.dynamics, weight: '25%', color: '#c8a97e', note: 'Love languages, conflict & stress response.' },
    { label: 'Complementarity', score: scores.complementarity, weight: '20%', color: '#8b6f9e', note: 'Balance of dominant & adaptive styles.' },
    { label: 'Lifestyle', score: scores.lifestyle, weight: '10%', color: '#7ab8d4', note: 'Day-to-day habits and living preferences.' },
    { label: 'Shared Interests', score: scores.interests, weight: '5%', color: '#d4846a', note: 'Overlap in core passions and hobbies.' },
  ];

  const resultsEl = document.getElementById('results-view');
  resultsEl.style.display = 'block';
  resultsEl.innerHTML = `
    <div class="result-hero fade-in">
      <div class="score-ring">
        <svg width="160" height="160" viewBox="0 0 160 160">
          <circle cx="80" cy="80" r="60" fill="none" stroke="#2e2b38" stroke-width="8"/>
          <circle cx="80" cy="80" r="60" fill="none" stroke="url(#scoreGrad)" stroke-width="8"
            stroke-dasharray="${filled} ${circumference - filled}" stroke-linecap="round"/>
          <defs>
            <linearGradient id="scoreGrad" x1="0%" y1="0%" x2="100%" y2="0%">
              <stop offset="0%" stop-color="#8b6f9e"/>
              <stop offset="100%" stop-color="#c8a97e"/>
            </linearGradient>
          </defs>
        </svg>
        <div class="score-text">
          <div class="score-number">${scores.total}</div>
          <div class="score-label">/ 100</div>
        </div>
      </div>
      <div class="result-title">${p1Name} & ${p2Name}: <em style="color:var(--accent);font-style:italic;">${title}</em></div>
      <div class="result-subtitle" style="color:var(--muted);font-size:14px;margin-top:8px;">Based on 32 questions across 4 compatibility layers</div>
    </div>

    <div style="background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);padding:20px 24px 24px;margin-bottom:32px;">
      <div style="font-size:11px;letter-spacing:0.1em;text-transform:uppercase;color:var(--muted);margin-bottom:16px;">Where you land</div>
      <div style="position:relative;margin-bottom:4px;">
        <div style="height:8px;border-radius:4px;background:linear-gradient(to right,#c0504a 0%,#d4846a 35%,#d4a86a 50%,#8bc87a 65%,#6db98a 80%,#c8a97e 100%);"></div>
        <div style="position:absolute;top:50%;left:${Math.min(Math.max(scores.total,2),98)}%;transform:translate(-50%,-50%);width:16px;height:16px;border-radius:50%;background:var(--bg);border:2.5px solid #fff;box-shadow:0 0 0 3px rgba(255,255,255,0.2),0 2px 8px rgba(0,0,0,0.6);"></div>
      </div>
      <div style="position:relative;height:8px;margin-bottom:2px;">
        <div style="position:absolute;left:25%;transform:translateX(-50%);width:1px;height:8px;background:var(--border);"></div>
        <div style="position:absolute;left:50%;transform:translateX(-50%);width:1px;height:8px;background:var(--border);"></div>
        <div style="position:absolute;left:75%;transform:translateX(-50%);width:1px;height:8px;background:var(--border);"></div>
      </div>
      <div style="position:relative;height:14px;margin-bottom:16px;">
        <div style="position:absolute;left:0;font-size:10px;color:var(--muted);">0</div>
        <div style="position:absolute;left:25%;transform:translateX(-50%);font-size:10px;color:var(--muted);">25</div>
        <div style="position:absolute;left:50%;transform:translateX(-50%);font-size:10px;color:var(--muted);">50</div>
        <div style="position:absolute;left:75%;transform:translateX(-50%);font-size:10px;color:var(--muted);">75</div>
        <div style="position:absolute;right:0;font-size:10px;color:var(--muted);">100</div>
      </div>
      <div style="display:flex;">
        <div style="width:35%;font-size:10px;color:#d4846a;letter-spacing:0.04em;line-height:1.4;">Growth Edges<br>Ahead</div>
        <div style="width:15%;font-size:10px;color:#d4a86a;text-align:center;letter-spacing:0.04em;line-height:1.4;">Comp.<br>Diff.</div>
        <div style="width:15%;font-size:10px;color:#8bc87a;text-align:center;letter-spacing:0.04em;line-height:1.4;">Good<br>Foundation</div>
        <div style="width:15%;font-size:10px;color:#6db98a;text-align:center;letter-spacing:0.04em;line-height:1.4;">Strongly<br>Compat.</div>
        <div style="width:20%;font-size:10px;color:#c8a97e;text-align:right;letter-spacing:0.04em;line-height:1.4;">Deeply<br>Aligned</div>
      </div>
    </div>

    <div class="breakdown-grid">
      ${breakdowns.map((b, i) => `
        <div class="breakdown-card"${i === breakdowns.length - 1 && breakdowns.length % 2 !== 0 ? ' style="grid-column:1/-1;"' : ''}>
          <div class="bc-label">${b.label} <span style="color:var(--border)">·</span> ${b.weight}</div>
          <div class="bc-score" style="color:${b.color}">${b.score}<span style="font-size:16px;color:var(--muted)">%</span></div>
          <div class="bc-bar"><div class="bc-fill" style="width:${b.score}%;background:${b.color};"></div></div>
          <div class="bc-note">${b.note}</div>
        </div>
      `).join('')}
    </div>

    <h2 style="font-family:'Playfair Display',serif;font-size:20px;margin:32px 0 16px;">What the numbers suggest</h2>
    ${insights.map(i => `
      <div class="insight-card">
        <div class="insight-type ${i.type}">${i.type === 'strength' ? '✦ Strength' : '⚡ Tension to explore'}</div>
        <div class="insight-text">${i.text}</div>
      </div>
    `).join('')}

    <div style="background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);padding:24px;margin-top:24px;">
      <div style="font-size:11px;letter-spacing:0.12em;text-transform:uppercase;color:var(--muted);margin-bottom:10px;">A note on these results</div>
      <p style="font-size:14px;color:var(--muted);line-height:1.7;">
        These scores are a <em>conversation starter</em>, not a verdict. Research shows that how couples interact day-to-day predicts outcomes far better than any profile survey can. A low score in one area can be balanced by strengths elsewhere — and compatibility can always be intentionally built. Use these results to ask better questions of each other.
      </p>
    </div>

    <div style="text-align:center;margin-top:32px;">
      <button class="btn btn-primary" id="share-btn" onclick="shareResults()">Share Results ↗</button>
    </div>

    <div class="result-cta">
      <p>Want to retake the survey or test with another couple?</p>
      <button class="btn btn-secondary" onclick="location.reload()">Start over</button>
    </div>
  `;

  resultsEl.scrollIntoView({ behavior: 'smooth' });
}

// ══ DEBUG ════════════════════════════════════════════════════════════════════

function renderDebugPanel() {
  if (!DEBUG) return;
  let panel = document.getElementById('debug-panel');
  if (!panel) {
    panel = document.createElement('div');
    panel.id = 'debug-panel';
    panel.style.cssText = 'position:fixed;bottom:16px;right:16px;background:#1a1820;border:1px solid #c8a97e;border-radius:8px;padding:14px 16px;z-index:9999;font-family:"DM Sans",sans-serif;min-width:210px;box-shadow:0 4px 24px rgba(0,0,0,0.6);';
    panel.innerHTML = `
      <div style="font-size:10px;letter-spacing:0.12em;text-transform:uppercase;color:#c8a97e;margin-bottom:10px;">⚙ Debug</div>
      <div style="margin-bottom:8px;">
        <div style="font-size:11px;color:#7a7585;margin-bottom:4px;">Partner</div>
        <select id="dbg-partner" onchange="debugJump()" style="width:100%;background:#221f2a;border:1px solid #2e2b38;border-radius:6px;color:#e8e4f0;padding:5px 8px;font-size:13px;outline:none;">
          <option value="1">1 · ${p1Name}</option>
          <option value="2">2 · ${p2Name}</option>
        </select>
      </div>
      <div style="margin-bottom:10px;">
        <div style="font-size:11px;color:#7a7585;margin-bottom:4px;">Question</div>
        <select id="dbg-question" onchange="debugJump()" style="width:100%;background:#221f2a;border:1px solid #2e2b38;border-radius:6px;color:#e8e4f0;padding:5px 8px;font-size:13px;outline:none;">
          ${questions.map((q,i) => `<option value="${i}">Q${i+1} · ${q.id}</option>`).join('')}
        </select>
      </div>
      <button onclick="debugSkipToResults()" style="width:100%;background:#c8a97e;color:#0f0e11;border:none;border-radius:6px;padding:7px;font-size:12px;cursor:pointer;font-family:'DM Sans',sans-serif;font-weight:500;">Skip to Results →</button>
    `;
    document.body.appendChild(panel);
  }
  document.getElementById('dbg-partner').value = currentPartner;
  document.getElementById('dbg-question').value = currentQ;
}

function debugJump() {
  currentPartner = parseInt(document.getElementById('dbg-partner').value);
  currentQ = parseInt(document.getElementById('dbg-question').value);
  renderQuestion();
}

function debugSkipToResults() {
  document.getElementById('survey-view').style.display = 'none';
  showResults();
}

if (DEBUG) {
  document.getElementById('name1').value = 'Alex';
  document.getElementById('name2').value = 'Jordan';
  startSurvey();
}
</script>
</body>
</html>
