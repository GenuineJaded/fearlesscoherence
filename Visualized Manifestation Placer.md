\<\!-- Visualized Manifestation Placer  
     receives a called zone.  
     chooses one background image from that zone's pool.  
     stretches it across the full canvas behind everything else.  
     opacity rolls within a narrow band (0.33–0.44).  
     shifts between rooms, doesn't change pressure wildly. \--\>  
\<style\>  
  :root {  
    \--bg: \#0a0a0c;  
    \--panel: \#111114;  
    \--line: \#1d1d22;  
    \--muted: \#6a6a72;  
    \--text: \#d8d8db;  
    \--accent: \#e8e8eb;

    \--water: \#4aa3c7;  
    \--air:   \#c9d4dc;  
    \--fire:  \#d97443;  
    \--earth: \#6b8260;  
    \--meta:  \#9b7ec4;  
  }  
  \* { box-sizing: border-box; margin: 0; padding: 0; }  
  html, body {  
    background: var(--bg);  
    color: var(--text);  
    font-family: 'JetBrains Mono', 'IBM Plex Mono', ui-monospace, Menlo, monospace;  
    font-size: 13px;  
    line-height: 1.55;  
    min-height: 100vh;  
  }  
  .wrap {  
    max-width: 1100px;  
    margin: 0 auto;  
    padding: 32px 24px 80px;  
  }  
  header {  
    border-bottom: 1px solid var(--line);  
    padding-bottom: 18px;  
    margin-bottom: 24px;  
  }  
  h1 {  
    font-size: 14px;  
    font-weight: 500;  
    letter-spacing: 0.5px;  
    color: var(--accent);  
    margin-bottom: 4px;  
  }  
  .sub { color: var(--muted); font-size: 12px; }

  .controls {  
    display: flex;  
    gap: 8px;  
    flex-wrap: wrap;  
    margin-bottom: 18px;  
  }  
  button {  
    background: var(--panel);  
    color: var(--text);  
    border: 1px solid var(--line);  
    padding: 7px 14px;  
    font-family: inherit;  
    font-size: 12px;  
    cursor: pointer;  
    transition: background 120ms, border-color 120ms;  
  }  
  button:hover { background: \#18181c; border-color: \#2a2a30; }  
  button.primary { border-color: \#3a3a42; }

  .zone-chips {  
    display: flex;  
    gap: 6px;  
    flex-wrap: wrap;  
    margin-bottom: 24px;  
  }  
  .chip {  
    font-size: 11px;  
    text-transform: lowercase;  
    letter-spacing: 1px;  
    padding: 5px 12px;  
    border: 1px solid var(--line);  
    background: transparent;  
    color: var(--muted);  
    cursor: pointer;  
    transition: all 120ms;  
    font-family: inherit;  
  }  
  .chip:hover { color: var(--text); }  
  .chip.water:hover, .chip.water.active { color: var(--water); border-color: var(--water); }  
  .chip.air:hover,   .chip.air.active   { color: var(--air);   border-color: var(--air); }  
  .chip.fire:hover,  .chip.fire.active  { color: var(--fire);  border-color: var(--fire); }  
  .chip.earth:hover, .chip.earth.active { color: var(--earth); border-color: var(--earth); }  
  .chip.meta:hover,  .chip.meta.active  { color: var(--meta);  border-color: var(--meta); }

  .called {  
    font-size: 11px;  
    color: var(--muted);  
    margin-bottom: 14px;  
    letter-spacing: 1px;  
    text-transform: uppercase;  
  }  
  .called .zone-name { font-weight: 500; margin-left: 8px; }  
  .called.water .zone-name { color: var(--water); }  
  .called.air   .zone-name { color: var(--air); }  
  .called.fire  .zone-name { color: var(--fire); }  
  .called.earth .zone-name { color: var(--earth); }  
  .called.meta  .zone-name { color: var(--meta); }

  .room {  
    position: relative;  
    width: 100%;  
    aspect-ratio: 16 / 10;  
    background: \#000;  
    border: 1px solid var(--line);  
    margin-bottom: 24px;  
    overflow: hidden;  
  }

  /\* the background field — full canvas, behind everything \*/  
  .background {  
    position: absolute;  
    inset: 0;  
    background-size: cover;  
    background-position: center center;  
    background-repeat: no-repeat;  
    transition: opacity 800ms cubic-bezier(.4,.1,.2,1),  
                background-image 0ms;  
    z-index: 0;  
  }

  /\* placeholder cells (the four cells the other engines will populate)  
     drawn here just to verify the background sits behind them. \*/  
  .placeholder-cell {  
    position: absolute;  
    border: 1px dashed rgba(216, 216, 219, 0.18);  
    background: rgba(10, 10, 12, 0.35);  
    z-index: 1;  
    display: flex;  
    align-items: center;  
    justify-content: center;  
    color: var(--muted);  
    font-size: 10px;  
    letter-spacing: 1.5px;  
  }

  .info {  
    display: grid;  
    grid-template-columns: 1fr 1fr 1fr 1fr;  
    gap: 16px;  
    border: 1px solid var(--line);  
    background: var(--panel);  
    padding: 14px 16px;  
    font-size: 11.5px;  
  }  
  .info-block .label {  
    color: var(--muted);  
    text-transform: uppercase;  
    letter-spacing: 1px;  
    font-size: 10px;  
    margin-bottom: 4px;  
  }  
  .info-block .val {  
    color: var(--text);  
    word-break: break-all;  
  }  
  .info-block a {  
    color: var(--text);  
    text-decoration: none;  
    border-bottom: 1px dotted var(--muted);  
  }  
  .info-block a:hover { border-bottom-color: var(--text); }

  .empty {  
    grid-column: 1 / \-1;  
    text-align: center;  
    padding: 18px;  
    color: var(--muted);  
    font-size: 11px;  
  }

  .toggle-row {  
    display: flex;  
    gap: 14px;  
    align-items: center;  
    margin-bottom: 18px;  
    font-size: 11px;  
    color: var(--muted);  
  }  
  .toggle-row label {  
    cursor: pointer;  
    user-select: none;  
    display: flex;  
    gap: 6px;  
    align-items: center;  
  }  
  .toggle-row input\[type=checkbox\] {  
    accent-color: var(--text);  
  }  
\</style\>

\<div class="wrap"\>  
  \<header\>  
    \<h1\>// visualized manifestation placer\</h1\>  
    \<div class="sub"\>zone → one background · stretched to canvas · opacity rolls within 0.33–0.44\</div\>  
  \</header\>

  \<div class="controls"\>  
    \<button class="primary" id="callRandom"\>call random zone\</button\>  
    \<button id="recall"\>call again (same zone)\</button\>  
  \</div\>

  \<div class="zone-chips" id="zoneChips"\>\</div\>

  \<div class="toggle-row"\>  
    \<label\>\<input type="checkbox" id="showCells" checked\> show placeholder cells\</label\>  
  \</div\>

  \<div class="called" id="called"\>  
    \<span\>called zone:\</span\>\<span class="zone-name" id="calledZone"\>—\</span\>  
  \</div\>

  \<div class="room" id="room"\>  
    \<div class="background" id="bg"\>\</div\>  
    \<\!-- placeholder cells injected here \--\>  
  \</div\>

  \<div class="info" id="info"\>  
    \<div class="empty"\>awaiting first call\</div\>  
  \</div\>  
\</div\>

\<script\>  
  // ─── corpus: backgrounds ────────────────────────────────────────────────  
  const ZONES \= \['water', 'air', 'fire', 'earth', 'meta'\];  
  const CDN \= 'https://cryptic-syndicate.com/wp-content/uploads/2026/04/';

  const bgFiles \= \[  
    'Mis1.jpg','Mis3-scaled.jpg','Mus2.jpeg','Mus4-scaled.png','Mus5.png',  
    'Mus6-scaled.png','Mus7-scaled.png','Mus8.png','Mus9-scaled.png','Mus10-scaled.png',  
    'Mus11-scaled.png','Mus12-scaled.png','Mus13-scaled.jpg','Mus14.jpg','Mus15.jpg',  
    'Mus16.jpg','Mus18.jpg','Mus19.jpg','Mus20.jpg','ThinkOfNothing.jpg'  
  \];

  const rollZone \= () \=\> ZONES\[Math.floor(Math.random() \* ZONES.length)\];  
  const backgrounds \= bgFiles.map(f \=\> ({  
    id: f,  
    name: f.replace(/\\.(jpg|jpeg|png)$/, ''),  
    src: CDN \+ f,  
    zone: rollZone()  
  }));

  // ─── the engine ─────────────────────────────────────────────────────────  
  // input:  zone (string), recentIds (array of last-N picked image ids)  
  // action: pick one background from the zone's pool, prefer something  
  //         not in recent memory; roll an opacity inside 0.33–0.44.  
  // output: { zone, item, opacity }

  const OPACITY\_MIN \= 0.33;  
  const OPACITY\_MAX \= 0.44;

  const recent \= \[\]; // memory window so we don't repeat backgrounds back-to-back  
  const RECENT\_MAX \= 3;

  function rollOpacity() {  
    return OPACITY\_MIN \+ Math.random() \* (OPACITY\_MAX \- OPACITY\_MIN);  
  }

  function visualizedManifestationPlacer(zone) {  
    const pool \= backgrounds.filter(b \=\> b.zone \=== zone);  
    // prefer something fresh; if pool is small enough that everything's recent, fall back  
    const fresh \= pool.filter(b \=\> \!recent.includes(b.id));  
    const pickFrom \= fresh.length ? fresh : pool;  
    const item \= pickFrom\[Math.floor(Math.random() \* pickFrom.length)\];

    recent.push(item.id);  
    if (recent.length \> RECENT\_MAX) recent.shift();

    return {  
      zone,  
      item,  
      opacity: rollOpacity(),  
      poolSize: pool.length  
    };  
  }

  // ─── render ─────────────────────────────────────────────────────────────  
  let last \= null;

  function renderRoom(result) {  
    const bg \= document.getElementById('bg');  
    if (\!result) {  
      bg.style.opacity \= 0;  
      bg.style.backgroundImage \= 'none';  
      return;  
    }  
    // crossfade: drop opacity, swap image, raise to new opacity.  
    bg.style.opacity \= 0;  
    setTimeout(() \=\> {  
      bg.style.backgroundImage \= \`url("${result.item.src}")\`;  
      // small delay so the browser registers the image change before the fade-in  
      requestAnimationFrame(() \=\> {  
        bg.style.opacity \= result.opacity;  
      });  
    }, 320);  
  }

  function renderInfo(result) {  
    const info \= document.getElementById('info');  
    if (\!result) {  
      info.innerHTML \= '\<div class="empty"\>awaiting first call\</div\>';  
      return;  
    }  
    info.innerHTML \= \`  
      \<div class="info-block"\>  
        \<div class="label"\>picked\</div\>  
        \<div class="val"\>\<a href="${result.item.src}" target="\_blank" rel="noopener"\>${result.item.name}\</a\>\</div\>  
      \</div\>  
      \<div class="info-block"\>  
        \<div class="label"\>opacity\</div\>  
        \<div class="val"\>${result.opacity.toFixed(3)}\</div\>  
      \</div\>  
      \<div class="info-block"\>  
        \<div class="label"\>zone\</div\>  
        \<div class="val"\>${result.zone}\</div\>  
      \</div\>  
      \<div class="info-block"\>  
        \<div class="label"\>pool\</div\>  
        \<div class="val"\>${result.poolSize} in zone\</div\>  
      \</div\>  
    \`;  
  }

  function renderCalled(result) {  
    const el \= document.getElementById('called');  
    el.className \= 'called' \+ (result ? ' ' \+ result.zone : '');  
    document.getElementById('calledZone').textContent \= result ? result.zone : '—';  
  }

  function renderChipsActive(zone) {  
    document.querySelectorAll('.chip').forEach(c \=\> {  
      c.classList.toggle('active', c.dataset.zone \=== zone);  
    });  
  }

  function call(zone) {  
    last \= visualizedManifestationPlacer(zone);  
    renderCalled(last);  
    renderRoom(last);  
    renderInfo(last);  
    renderChipsActive(zone);  
  }

  // ─── placeholder cells (just to verify the background sits behind) ─────  
  function renderPlaceholderCells(show) {  
    const room \= document.getElementById('room');  
    // remove existing placeholders  
    \[...room.querySelectorAll('.placeholder-cell')\].forEach(el \=\> el.remove());  
    if (\!show) return;  
    // four representative cells — these are not the real layout, just markers  
    const cells \= \[  
      { x:  4, y:  4, w: 56, h: 18, label: 'audio' },  
      { x:  4, y: 26, w: 28, h: 70, label: '2' },  
      { x: 34, y: 26, w: 26, h: 32, label: '3' },  
      { x: 34, y: 60, w: 26, h: 36, label: '4' }  
    \];  
    cells.forEach(c \=\> {  
      const el \= document.createElement('div');  
      el.className \= 'placeholder-cell';  
      el.style.left \= c.x \+ '%';  
      el.style.top  \= c.y \+ '%';  
      el.style.width  \= c.w \+ '%';  
      el.style.height \= c.h \+ '%';  
      el.textContent \= c.label;  
      room.appendChild(el);  
    });  
  }

  // ─── chips & controls ───────────────────────────────────────────────────  
  const chipsEl \= document.getElementById('zoneChips');  
  ZONES.forEach(z \=\> {  
    const b \= document.createElement('button');  
    b.className \= 'chip ' \+ z;  
    b.dataset.zone \= z;  
    b.textContent \= z;  
    b.addEventListener('click', () \=\> call(z));  
    chipsEl.appendChild(b);  
  });

  document.getElementById('callRandom').addEventListener('click', () \=\> call(rollZone()));  
  document.getElementById('recall').addEventListener('click', () \=\> call(last ? last.zone : rollZone()));  
  document.getElementById('showCells').addEventListener('change', e \=\> {  
    renderPlaceholderCells(e.target.checked);  
  });

  // ─── init ───────────────────────────────────────────────────────────────  
  renderPlaceholderCells(true);  
  renderInfo(null);  
\</script\>