\<\!-- Paradox Zone Navigator  
     two functions:  
       1\. tags every piece of atmospheric media (audio, video, background)  
          into one of: water · air · fire · earth · meta — one source of truth.  
       2\. calls the next zone when a room is about to arise — random with memory.

     output: the manifest other engines read from, and the called zone. \--\>  
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

    \--water-bg: rgba(74, 163, 199, 0.08);  
    \--air-bg:   rgba(201, 212, 220, 0.06);  
    \--fire-bg:  rgba(217, 116, 67, 0.08);  
    \--earth-bg: rgba(107, 130, 96, 0.08);  
    \--meta-bg:  rgba(155, 126, 196, 0.08);  
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

  .section {  
    margin-bottom: 32px;  
  }  
  .section-head {  
    display: flex;  
    justify-content: space-between;  
    align-items: baseline;  
    border-bottom: 1px solid var(--line);  
    padding-bottom: 6px;  
    margin-bottom: 12px;  
  }  
  .section-head h2 {  
    font-size: 12px;  
    font-weight: 500;  
    color: var(--accent);  
    letter-spacing: 1px;  
    text-transform: uppercase;  
  }  
  .section-head .meta {  
    color: var(--muted);  
    font-size: 11px;  
  }

  /\* CALL panel — the second function \*/  
  .call-panel {  
    border: 1px solid var(--line);  
    background: var(--panel);  
    padding: 20px;  
    margin-bottom: 24px;  
  }  
  .call-row {  
    display: flex;  
    gap: 18px;  
    align-items: center;  
    margin-bottom: 14px;  
    flex-wrap: wrap;  
  }  
  .call-row .called-display {  
    font-size: 11px;  
    color: var(--muted);  
    text-transform: uppercase;  
    letter-spacing: 1px;  
  }  
  .called-zone-label {  
    font-size: 22px;  
    font-weight: 500;  
    margin-left: 12px;  
    letter-spacing: 2px;  
    text-transform: lowercase;  
  }  
  .called-zone-label.water { color: var(--water); }  
  .called-zone-label.air   { color: var(--air); }  
  .called-zone-label.fire  { color: var(--fire); }  
  .called-zone-label.earth { color: var(--earth); }  
  .called-zone-label.meta  { color: var(--meta); }  
  .called-zone-label.empty { color: var(--muted); font-size: 16px; }

  .history {  
    display: flex;  
    gap: 6px;  
    align-items: center;  
    flex-wrap: wrap;  
    font-size: 10px;  
    color: var(--muted);  
    text-transform: uppercase;  
    letter-spacing: 1px;  
  }  
  .history-item {  
    padding: 3px 9px;  
    border: 1px solid var(--line);  
    background: transparent;  
  }  
  .history-item.water { color: var(--water); border-color: rgba(74, 163, 199, 0.4); }  
  .history-item.air   { color: var(--air);   border-color: rgba(201, 212, 220, 0.3); }  
  .history-item.fire  { color: var(--fire);  border-color: rgba(217, 116, 67, 0.4); }  
  .history-item.earth { color: var(--earth); border-color: rgba(107, 130, 96, 0.4); }  
  .history-item.meta  { color: var(--meta);  border-color: rgba(155, 126, 196, 0.4); }  
  .history-label { color: var(--muted); }

  .controls {  
    display: flex;  
    gap: 8px;  
    flex-wrap: wrap;  
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
  button.big {  
    padding: 10px 20px;  
    font-size: 13px;  
  }

  .memory-control {  
    display: flex;  
    gap: 8px;  
    align-items: center;  
    font-size: 11px;  
    color: var(--muted);  
    margin-left: auto;  
  }  
  .memory-control input\[type=number\] {  
    background: var(--bg);  
    border: 1px solid var(--line);  
    color: var(--text);  
    padding: 4px 8px;  
    width: 50px;  
    font-family: inherit;  
    font-size: 11px;  
  }

  /\* TAGGING — the first function \*/  
  .counts {  
    display: flex;  
    gap: 14px;  
    flex-wrap: wrap;  
    padding: 14px 16px;  
    border: 1px solid var(--line);  
    background: var(--panel);  
    margin-bottom: 24px;  
  }  
  .count {  
    display: flex;  
    align-items: baseline;  
    gap: 8px;  
    font-size: 12px;  
  }  
  .count .swatch {  
    width: 8px; height: 8px;  
    display: inline-block;  
    border-radius: 1px;  
  }  
  .count .label { color: var(--muted); text-transform: lowercase; letter-spacing: 0.5px; }  
  .count .num   { color: var(--text); min-width: 24px; font-variant-numeric: tabular-nums; }  
  .swatch.water { background: var(--water); }  
  .swatch.air   { background: var(--air); }  
  .swatch.fire  { background: var(--fire); }  
  .swatch.earth { background: var(--earth); }  
  .swatch.meta  { background: var(--meta); }

  .grid {  
    display: grid;  
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));  
    gap: 1px;  
    background: var(--line);  
    border: 1px solid var(--line);  
  }  
  .item {  
    background: var(--panel);  
    padding: 10px 12px;  
    display: flex;  
    align-items: center;  
    gap: 10px;  
    cursor: pointer;  
    transition: background 100ms;  
    overflow: hidden;  
    border-left: 2px solid transparent;  
  }  
  .item:hover { background: \#16161a; }  
  .item.water { border-left-color: var(--water); background: var(--water-bg); }  
  .item.air   { border-left-color: var(--air);   background: var(--air-bg); }  
  .item.fire  { border-left-color: var(--fire);  background: var(--fire-bg); }  
  .item.earth { border-left-color: var(--earth); background: var(--earth-bg); }  
  .item.meta  { border-left-color: var(--meta);  background: var(--meta-bg); }  
  .item .name {  
    flex: 1; overflow: hidden; text-overflow: ellipsis; white-space: nowrap;  
    font-size: 11.5px; color: var(--text);  
  }  
  .item .zone {  
    font-size: 10px; text-transform: uppercase; letter-spacing: 1px;  
    color: var(--muted); min-width: 44px; text-align: right;  
  }  
  .item.water .zone { color: var(--water); }  
  .item.air   .zone { color: var(--air); }  
  .item.fire  .zone { color: var(--fire); }  
  .item.earth .zone { color: var(--earth); }  
  .item.meta  .zone { color: var(--meta); }

  .manifest-panel {  
    margin-top: 32px;  
    border: 1px solid var(--line);  
    background: var(--panel);  
  }  
  .manifest-head {  
    display: flex;  
    justify-content: space-between;  
    align-items: center;  
    padding: 10px 14px;  
    border-bottom: 1px solid var(--line);  
  }  
  .manifest-head h2 {  
    font-size: 11px;  
    font-weight: 500;  
    color: var(--accent);  
    letter-spacing: 1px;  
    text-transform: uppercase;  
  }  
  .manifest-head .actions { display: flex; gap: 6px; }  
  .manifest-head button { padding: 4px 10px; font-size: 11px; }  
  pre {  
    padding: 14px;  
    font-size: 11px;  
    line-height: 1.5;  
    color: \#a8a8ad;  
    overflow-x: auto;  
    max-height: 360px;  
    overflow-y: auto;  
    white-space: pre;  
    font-family: inherit;  
  }  
  .hint { color: var(--muted); font-size: 11px; margin-top: 8px; }  
  .toast {  
    position: fixed;  
    bottom: 24px;  
    left: 50%;  
    transform: translateX(-50%);  
    background: var(--panel);  
    border: 1px solid var(--line);  
    padding: 8px 16px;  
    font-size: 11px;  
    color: var(--text);  
    opacity: 0;  
    transition: opacity 200ms;  
    pointer-events: none;  
  }  
  .toast.show { opacity: 1; }

  .reset-bar {  
    display: flex;  
    gap: 8px;  
    margin-bottom: 18px;  
    flex-wrap: wrap;  
  }  
\</style\>

\<div class="wrap"\>  
  \<header\>  
    \<h1\>// paradox zone navigator\</h1\>  
    \<div class="sub"\>tags atmospheric media · calls the next zone · random with memory\</div\>  
  \</header\>

  \<\!-- ═══ FUNCTION TWO: CALL ═══════════════════════════════════════════════ \--\>  
  \<div class="section"\>  
    \<div class="section-head"\>  
      \<h2\>call\</h2\>  
      \<div class="meta"\>the next room arises from\</div\>  
    \</div\>  
    \<div class="call-panel"\>  
      \<div class="call-row"\>  
        \<div class="called-display"\>  
          \<span\>called zone:\</span\>  
          \<span class="called-zone-label empty" id="calledZone"\>—\</span\>  
        \</div\>  
        \<div class="memory-control"\>  
          \<span\>memory window:\</span\>  
          \<input type="number" id="memorySize" min="0" max="4" value="2"\>  
          \<span\>last calls remembered\</span\>  
        \</div\>  
      \</div\>  
      \<div class="call-row"\>  
        \<div class="history"\>  
          \<span class="history-label"\>history:\</span\>  
          \<span id="historyList"\>\<span style="color: var(--muted);"\>none yet\</span\>\</span\>  
        \</div\>  
      \</div\>  
      \<div class="controls"\>  
        \<button class="primary big" id="callNext"\>call next zone\</button\>  
        \<button id="resetHistory"\>clear history\</button\>  
      \</div\>  
    \</div\>  
  \</div\>

  \<\!-- ═══ FUNCTION ONE: TAG ════════════════════════════════════════════════ \--\>  
  \<div class="section"\>  
    \<div class="section-head"\>  
      \<h2\>tag\</h2\>  
      \<div class="meta"\>elemental belonging · one source of truth\</div\>  
    \</div\>

    \<div class="reset-bar"\>  
      \<button class="primary" id="rerollAll"\>re-roll all\</button\>  
      \<button id="copyManifest"\>copy manifest\</button\>  
      \<button id="downloadManifest"\>download manifest.json\</button\>  
    \</div\>

    \<div class="counts" id="counts"\>\</div\>  
  \</div\>

  \<div class="section"\>  
    \<div class="section-head"\>  
      \<h2\>audio · suno\</h2\>  
      \<div class="meta" id="audioCount"\>\</div\>  
    \</div\>  
    \<div class="grid" id="audioGrid"\>\</div\>  
    \<div class="hint"\>click any item to re-roll its zone\</div\>  
  \</div\>

  \<div class="section"\>  
    \<div class="section-head"\>  
      \<h2\>video\</h2\>  
      \<div class="meta" id="videoCount"\>\</div\>  
    \</div\>  
    \<div class="grid" id="videoGrid"\>\</div\>  
  \</div\>

  \<div class="section"\>  
    \<div class="section-head"\>  
      \<h2\>background\</h2\>  
      \<div class="meta" id="bgCount"\>\</div\>  
    \</div\>  
    \<div class="grid" id="bgGrid"\>\</div\>  
  \</div\>

  \<div class="manifest-panel"\>  
    \<div class="manifest-head"\>  
      \<h2\>manifest.json\</h2\>  
      \<div class="actions"\>  
        \<button id="copyManifest2"\>copy\</button\>  
      \</div\>  
    \</div\>  
    \<pre id="manifestOut"\>\</pre\>  
  \</div\>  
\</div\>

\<div class="toast" id="toast"\>\</div\>

\<script\>  
  // ─── corpus ─────────────────────────────────────────────────────────────  
  const ZONES \= \['water', 'air', 'fire', 'earth', 'meta'\];  
  const SUNO\_BASE \= 'https://suno.com/song/';  
  const CDN \= 'https://cryptic-syndicate.com/wp-content/uploads/2026/04/';

  const audio \= \[  
    '71977aba-d79b-4178-95b9-8a238d2f1180','3802c87a-fd70-42e0-adda-fea259575485',  
    'bd355dba-7754-4fa0-b717-d5b00aafc182','e83a28fa-3eb2-4405-a877-0aac2cdcf470',  
    '7c0e37d4-df16-4c65-8a4b-55938877c280','7c6d24aa-e36a-49e2-a5fe-e4440e791fb2',  
    '536f5162-8a06-43a6-a1db-67c2b6eebe83','4147d940-9be9-4b3b-a9f6-6c2c9d1fed76',  
    '640650d6-5b31-42fa-bc95-e381b0bc9a5c','213556e5-c413-43fc-937f-821d260c9271',  
    '307380a4-34c9-4593-bb07-7f6af5493f66','63886711-d631-47db-ad69-c333122d2fef',  
    'b33f4f72-da77-450b-9aab-3fc8cf0e9768','66a36451-0e8a-4bc5-b10d-e1712ad509cd',  
    'ee668425-2745-41d7-9db4-aaa57ae37853','da542666-fbb0-4428-a316-8f2f07aeb99a',  
    '8a9c17d4-055e-46ee-9fb6-46d68379c687','b48af77c-7a5d-4c3a-8896-349415a18cbb',  
    '20396ca9-818c-4ccf-bf14-d7846a8aee11','27c97f41-7204-45f8-9466-c1d5209a38e6',  
    '634a3945-d5fe-4671-95ec-26ed1bd85830','7cd0001c-13b9-4ab5-b93b-73c62d327c2f',  
    '97467829-e201-4588-8db6-cb1544efea3b','1b999d6e-ae9c-424d-813f-1c0db02483d4',  
    '0cf7a462-0a43-48cb-b956-2ab1616bb78d','47be2129-fafa-4f2d-857b-23399e66aca7',  
    '7a9a2fbf-cc93-45ad-9fd5-232ef323e980','d9507113-e293-4649-b2ba-b89e8a84217b',  
    'cdbc9de2-cc61-4b86-a1a7-240cffaf3e4b','fb026def-e30e-437b-ab1f-76e2946d98d4',  
    'c6edd450-52d2-45c8-b1b1-d75c158423df','03bc9d13-cdf9-4a4b-a936-d598c3b22921',  
    '337368dc-366a-472d-a8a2-11f55319b928','ae0644ce-e003-4e56-9ac4-adc6dbc4c9c6',  
    '7fb93e18-883d-426e-9049-227ec1d268ca','44b1fbf8-c498-4509-a494-d8f483201720',  
    '66bf4e41-b7f3-44a2-bb8f-a189e8a25803','92ddc110-575f-4077-a4a7-109d58d3d0d2',  
    'da6d6dd0-9dd9-4066-ba57-40d1b4214605','b6fe928a-de7b-4dc7-b840-824a89a5d2c7',  
    '250fcace-d5c9-452b-ba30-470a50dbeaed','96a67405-ce92-42d1-bba9-1fcff91e9266',  
    '9c9ca07a-60ff-4ac2-8085-504c5f7226e4','0754d6aa-0039-4e4a-b45f-09af83efd08e',  
    '1d91824b-c7d5-4a6b-a298-aa3228d4abcc','c2744e5f-d051-4d23-9f70-e2b90dc26d58',  
    '4af36f5e-2116-43de-9004-9a3cd52ec459'  
  \].map(id \=\> ({ kind: 'audio', id, src: SUNO\_BASE \+ id, name: id.slice(0, 8), zone: null }));

  const videoNames \= \[  
    'AmazingBlazingMusing','AmusingMaze','AteSnax','CheckaDough','GanGain',  
    'GrimmSales','Hrmph','InterestingTho','NotMeProbly','Trinne','TrippleTrial'  
  \];  
  const video \= videoNames.map(n \=\> ({  
    kind: 'video', id: n, src: CDN \+ n \+ '.mp4', name: n, zone: null  
  }));

  const bgFiles \= \[  
    'Mis1.jpg','Mis3-scaled.jpg','Mus2.jpeg','Mus4-scaled.png','Mus5.png',  
    'Mus6-scaled.png','Mus7-scaled.png','Mus8.png','Mus9-scaled.png','Mus10-scaled.png',  
    'Mus11-scaled.png','Mus12-scaled.png','Mus13-scaled.jpg','Mus14.jpg','Mus15.jpg',  
    'Mus16.jpg','Mus18.jpg','Mus19.jpg','Mus20.jpg','ThinkOfNothing.jpg'  
  \];  
  const background \= bgFiles.map(f \=\> ({  
    kind: 'background', id: f, src: CDN \+ f, name: f.replace(/\\.(jpg|jpeg|png)$/, ''), zone: null  
  }));

  const corpus \= { audio, video, background };

  // ─── FUNCTION ONE: tag ──────────────────────────────────────────────────  
  function rollZone() { return ZONES\[Math.floor(Math.random() \* ZONES.length)\]; }  
  function tagAll(items) { items.forEach(it \=\> it.zone \= rollZone()); }

  // ─── FUNCTION TWO: call (random with memory) ────────────────────────────  
  // input:  recent (array of last N called zones), memorySize (N)  
  // action: pick a zone not in recent memory; if memory has saturated all  
  //         zones, fall back to the full set (this only happens if memorySize  
  //         is set so high it equals or exceeds the number of zones).  
  // output: the called zone (string)  
  const history \= \[\];

  function callNextZone(memorySize) {  
    const recent \= history.slice(-memorySize);  
    const fresh \= ZONES.filter(z \=\> \!recent.includes(z));  
    const pickFrom \= fresh.length ? fresh : ZONES;  
    const next \= pickFrom\[Math.floor(Math.random() \* pickFrom.length)\];  
    history.push(next);  
    return next;  
  }

  // ─── render: tag ────────────────────────────────────────────────────────  
  function renderGrid(elId, items) {  
    const grid \= document.getElementById(elId);  
    grid.innerHTML \= '';  
    items.forEach((it) \=\> {  
      const div \= document.createElement('div');  
      div.className \= 'item' \+ (it.zone ? ' ' \+ it.zone : '');  
      div.innerHTML \= \`  
        \<span class="name" title="${it.src}"\>${it.name}\</span\>  
        \<span class="zone"\>${it.zone || '—'}\</span\>  
      \`;  
      div.addEventListener('click', () \=\> {  
        let next \= rollZone();  
        if (it.zone) {  
          let tries \= 0;  
          while (next \=== it.zone && tries \< 4\) { next \= rollZone(); tries++; }  
        }  
        it.zone \= next;  
        renderTagging();  
      });  
      grid.appendChild(div);  
    });  
  }

  function renderCounts() {  
    const all \= \[...corpus.audio, ...corpus.video, ...corpus.background\];  
    const tally \= Object.fromEntries(ZONES.map(z \=\> \[z, 0\]));  
    all.forEach(it \=\> { if (it.zone) tally\[it.zone\]++; });  
    const el \= document.getElementById('counts');  
    el.innerHTML \= ZONES.map(z \=\> \`  
      \<div class="count"\>  
        \<span class="swatch ${z}"\>\</span\>  
        \<span class="label"\>${z}\</span\>  
        \<span class="num"\>${tally\[z\]}\</span\>  
      \</div\>  
    \`).join('');

    document.getElementById('audioCount').textContent \= \`${corpus.audio.length} items\`;  
    document.getElementById('videoCount').textContent \= \`${corpus.video.length} items\`;  
    document.getElementById('bgCount').textContent    \= \`${corpus.background.length} items\`;  
  }

  function buildManifest() {  
    return {  
      generated\_at: new Date().toISOString(),  
      zones: ZONES,  
      counts: ZONES.reduce((acc, z) \=\> {  
        acc\[z\] \= \[...corpus.audio, ...corpus.video, ...corpus.background\]  
          .filter(it \=\> it.zone \=== z).length;  
        return acc;  
      }, {}),  
      audio:      corpus.audio.map(it      \=\> ({ id: it.id, src: it.src, zone: it.zone })),  
      video:      corpus.video.map(it      \=\> ({ id: it.id, src: it.src, zone: it.zone })),  
      background: corpus.background.map(it \=\> ({ id: it.id, src: it.src, zone: it.zone }))  
    };  
  }

  function renderManifest() {  
    document.getElementById('manifestOut').textContent \=  
      JSON.stringify(buildManifest(), null, 2);  
  }

  function renderTagging() {  
    renderGrid('audioGrid', corpus.audio);  
    renderGrid('videoGrid', corpus.video);  
    renderGrid('bgGrid',    corpus.background);  
    renderCounts();  
    renderManifest();  
  }

  // ─── render: call ───────────────────────────────────────────────────────  
  function renderCalled(zone) {  
    const el \= document.getElementById('calledZone');  
    el.className \= 'called-zone-label ' \+ (zone || 'empty');  
    el.textContent \= zone || '—';  
  }

  function renderHistory() {  
    const el \= document.getElementById('historyList');  
    if (history.length \=== 0\) {  
      el.innerHTML \= '\<span style="color: var(--muted);"\>none yet\</span\>';  
      return;  
    }  
    // most recent on the right  
    el.innerHTML \= history.slice(-10).map(z \=\>  
      \`\<span class="history-item ${z}"\>${z}\</span\>\`  
    ).join('');  
  }

  // ─── controls ───────────────────────────────────────────────────────────  
  function toast(msg) {  
    const t \= document.getElementById('toast');  
    t.textContent \= msg;  
    t.classList.add('show');  
    clearTimeout(t.\_h);  
    t.\_h \= setTimeout(() \=\> t.classList.remove('show'), 1400);  
  }

  document.getElementById('callNext').addEventListener('click', () \=\> {  
    const mem \= parseInt(document.getElementById('memorySize').value) || 0;  
    const zone \= callNextZone(mem);  
    renderCalled(zone);  
    renderHistory();  
  });

  document.getElementById('resetHistory').addEventListener('click', () \=\> {  
    history.length \= 0;  
    renderHistory();  
    renderCalled(null);  
    toast('history cleared');  
  });

  document.getElementById('rerollAll').addEventListener('click', () \=\> {  
    \[corpus.audio, corpus.video, corpus.background\].forEach(tagAll);  
    renderTagging();  
    toast('all re-rolled');  
  });

  function copyManifest() {  
    const txt \= JSON.stringify(buildManifest(), null, 2);  
    navigator.clipboard.writeText(txt).then(() \=\> toast('manifest copied'));  
  }  
  document.getElementById('copyManifest').addEventListener('click', copyManifest);  
  document.getElementById('copyManifest2').addEventListener('click', copyManifest);

  document.getElementById('downloadManifest').addEventListener('click', () \=\> {  
    const txt \= JSON.stringify(buildManifest(), null, 2);  
    const blob \= new Blob(\[txt\], { type: 'application/json' });  
    const url \= URL.createObjectURL(blob);  
    const a \= document.createElement('a');  
    a.href \= url;  
    a.download \= 'manifest.json';  
    a.click();  
    URL.revokeObjectURL(url);  
    toast('manifest downloaded');  
  });

  // ─── init ───────────────────────────────────────────────────────────────  
  \[corpus.audio, corpus.video, corpus.background\].forEach(tagAll);  
  renderTagging();  
  renderHistory();  
  renderCalled(null);  
\</script\>