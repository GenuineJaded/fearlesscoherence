\<\!-- Sonic Sorting System  
     receives a called zone.  
     picks one auditory presence (suno or video).  
     shapes its cell from the medium's natural ratio.  
     anchors the cell on the canvas.  
     subdivides the leftover region into three cells.  
     embeds correctly. places. \--\>  
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

  /\* the room is now a positioned canvas; cells are absolutely placed \*/  
  .room {  
    position: relative;  
    width: 100%;  
    aspect-ratio: 16 / 10;  
    background: var(--line);  
    border: 1px solid var(--line);  
    margin-bottom: 24px;  
    overflow: hidden;  
  }  
  .cell {  
    position: absolute;  
    background: var(--panel);  
    overflow: hidden;  
    transition: box-shadow 200ms, background 200ms,  
                left 320ms cubic-bezier(.4,.1,.2,1),  
                top 320ms cubic-bezier(.4,.1,.2,1),  
                width 320ms cubic-bezier(.4,.1,.2,1),  
                height 320ms cubic-bezier(.4,.1,.2,1);  
    display: flex;  
    align-items: center;  
    justify-content: center;  
  }  
  .cell .placeholder {  
    color: var(--muted);  
    font-size: 10px;  
    letter-spacing: 1.5px;  
    opacity: 0.4;  
  }  
  .cell.audio.water { box-shadow: inset 0 0 0 1px var(--water); background: rgba(74, 163, 199, 0.04); }  
  .cell.audio.air   { box-shadow: inset 0 0 0 1px var(--air);   background: rgba(201, 212, 220, 0.04); }  
  .cell.audio.fire  { box-shadow: inset 0 0 0 1px var(--fire);  background: rgba(217, 116, 67, 0.04); }  
  .cell.audio.earth { box-shadow: inset 0 0 0 1px var(--earth); background: rgba(107, 130, 96, 0.04); }  
  .cell.audio.meta  { box-shadow: inset 0 0 0 1px var(--meta);  background: rgba(155, 126, 196, 0.04); }

  .cell iframe, .cell video {  
    width: 100%;  
    height: 100%;  
    border: 0;  
    background: \#000;  
    display: block;  
  }  
  .cell video { object-fit: contain; }

  .cell .cell-num {  
    position: absolute;  
    top: 6px;  
    left: 8px;  
    font-size: 9px;  
    letter-spacing: 1.5px;  
    color: var(--muted);  
    z-index: 2;  
    pointer-events: none;  
  }  
  .cell.audio .cell-num {  
    background: rgba(10, 10, 12, 0.7);  
    padding: 2px 6px;  
    color: var(--text);  
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
\</style\>

\<div class="wrap"\>  
  \<header\>  
    \<h1\>// sonic sorting system\</h1\>  
    \<div class="sub"\>zone → audio · suno or video · cell shaped from medium · anchored · leftover subdivided into three\</div\>  
  \</header\>

  \<div class="controls"\>  
    \<button class="primary" id="callRandom"\>call random zone\</button\>  
    \<button id="recall"\>call again (same zone)\</button\>  
  \</div\>

  \<div class="zone-chips" id="zoneChips"\>\</div\>

  \<div class="called" id="called"\>  
    \<span\>called zone:\</span\>\<span class="zone-name" id="calledZone"\>—\</span\>  
  \</div\>

  \<div class="room" id="room"\>\</div\>

  \<div class="info" id="info"\>  
    \<div class="empty"\>awaiting first call\</div\>  
  \</div\>  
\</div\>

\<script\>  
  // ─── corpus ─────────────────────────────────────────────────────────────  
  const ZONES \= \['water', 'air', 'fire', 'earth', 'meta'\];  
  const SUNO\_EMBED \= 'https://suno.com/embed/';  
  const SUNO\_PAGE  \= 'https://suno.com/song/';  
  const CDN \= 'https://cryptic-syndicate.com/wp-content/uploads/2026/04/';

  const audioIds \= \[  
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
  \];

  const videoNames \= \[  
    'AmazingBlazingMusing','AmusingMaze','AteSnax','CheckaDough','GanGain',  
    'GrimmSales','Hrmph','InterestingTho','NotMeProbly','Trinne','TrippleTrial'  
  \];

  const rollZone \= () \=\> ZONES\[Math.floor(Math.random() \* ZONES.length)\];  
  const corpus \= {  
    audio: audioIds.map(id \=\> ({  
      kind: 'audio', id, name: id.slice(0, 8), zone: rollZone(),  
      embed: SUNO\_EMBED \+ id, page: SUNO\_PAGE \+ id  
    })),  
    video: videoNames.map(n \=\> ({  
      kind: 'video', id: n, name: n, zone: rollZone(),  
      src: CDN \+ n \+ '.mp4'  
    }))  
  };

  // ─── geometry: medium-driven cell shaping \+ leftover subdivision ────────  
  // canvas is normalized to a unit rectangle; the room CSS aspect-ratio  
  // (16:10 here) maps the unit to pixels. cells are emitted as  
  // { x, y, w, h } in \[0..1\].  
  //  
  // suno embeds are 760×240 → ratio 19:6 ≈ 3.17.  
  // music videos vary; treated as \~16:9 ≈ 1.78 unless we know better.  
  //  
  // the canvas itself has an aspect ratio (canvasAR \= canvasW / canvasH).  
  // when fitting a cell of ratio \`r\` and area-fraction \`a\` into the canvas,  
  // the cell's normalized width and height come from:  
  //  
  //   cellW\_px / cellH\_px \= r              (preserve medium ratio)  
  //   cellW\_norm \= cellW\_px / canvasW  
  //   cellH\_norm \= cellH\_px / canvasH  
  //   so cellW\_norm / cellH\_norm \= r \* (canvasH / canvasW) \= r / canvasAR  
  //   and cellW\_norm \* cellH\_norm \= a    (area-fraction of canvas)  
  //  
  //   → cellH\_norm \= sqrt(a \* canvasAR / r)  
  //   → cellW\_norm \= a / cellH\_norm  
  //  
  // anchor decides which edge the cell hugs. a wide cell (r \>\> canvasAR)  
  // can only hug top or bottom (it's wider than the canvas is tall enough  
  // to allow as a column). a tall-ish cell can hug any edge or float.

  const CANVAS\_AR \= 16 / 10;        // matches the room CSS  
  const SUNO\_RATIO \= 760 / 240;     // 3.166...  
  const VIDEO\_RATIO \= 16 / 9;       // generic assumption

  const ANCHORS \= \['top', 'bottom', 'left', 'right', 'float'\];

  function ratioFor(kind) {  
    return kind \=== 'audio' ? SUNO\_RATIO : VIDEO\_RATIO;  
  }

  // pick area-fraction within a kind-appropriate range. suno banners read  
  // best when they get real width, so they claim a bit more area on average.  
  function rollScale(kind) {  
    const range \= kind \=== 'audio' ? \[0.34, 0.50\] : \[0.30, 0.46\];  
    return range\[0\] \+ Math.random() \* (range\[1\] \- range\[0\]);  
  }

  // figure out which anchors are physically valid given the cell's  
  // normalized width/height. wider-than-tall cells can't sit as a column  
  // unless their height is reasonable; floating needs slack on every side.  
  function validAnchors(cellW, cellH) {  
    const valid \= \[\];  
    // top/bottom: the cell needs to fit horizontally (always true since  
    // we cap below) and leave at least \~25% canvas height for the rest.  
    if (cellW \<= 1.0 && cellH \<= 0.72) {  
      valid.push('top', 'bottom');  
    }  
    // left/right: the cell needs to fit vertically as a column.  
    // a column makes sense when the cell isn't pathologically wide.  
    if (cellH \<= 1.0 && cellW \<= 0.72) {  
      valid.push('left', 'right');  
    }  
    // float: needs margin on every side; cell must be smaller in both axes.  
    if (cellW \<= 0.65 && cellH \<= 0.70) {  
      valid.push('float');  
    }  
    return valid;  
  }

  // shape and anchor the audio cell.  
  function shapeAudioCell(kind, recentAnchors) {  
    const r \= ratioFor(kind);  
    let attempts \= 0;  
    while (attempts \< 12\) {  
      const a \= rollScale(kind);  
      // cellH\_norm \= sqrt(a \* canvasAR / r)  
      let cellH \= Math.sqrt((a \* CANVAS\_AR) / r);  
      let cellW \= a / cellH;

      // clamp: a Suno banner can compute a width \> 1; if so, shrink to fit  
      // and recompute height to preserve ratio.  
      if (cellW \> 0.96) {  
        cellW \= 0.96;  
        cellH \= cellW \* (CANVAS\_AR / r);  
      }  
      if (cellH \> 0.92) {  
        cellH \= 0.92;  
        cellW \= cellH \* (r / CANVAS\_AR);  
      }

      // physically valid anchors for this shape  
      let anchors \= validAnchors(cellW, cellH);  
      // alternation: prefer anchors not in recent memory; only fall back  
      // to the full set if everything's been used recently.  
      const fresh \= anchors.filter(x \=\> \!recentAnchors.includes(x));  
      const pickFrom \= fresh.length ? fresh : anchors;  
      if (pickFrom.length \=== 0\) { attempts++; continue; }

      const anchor \= pickFrom\[Math.floor(Math.random() \* pickFrom.length)\];  
      const rect \= placeAtAnchor(cellW, cellH, anchor);  
      return { rect, anchor, cellW, cellH };  
    }  
    // pathological fallback: a centered banner  
    return {  
      rect: { x: 0.05, y: 0.40, w: 0.90, h: 0.20 },  
      anchor: 'top', cellW: 0.90, cellH: 0.20  
    };  
  }

  // place a cell of given normalized w/h against its anchor, with a small  
  // gutter from the edge so cells don't touch the canvas border.  
  function placeAtAnchor(w, h, anchor) {  
    const G \= 0.012; // gutter  
    switch (anchor) {  
      case 'top':    return { x: (1 \- w) / 2, y: G, w, h };  
      case 'bottom': return { x: (1 \- w) / 2, y: 1 \- h \- G, w, h };  
      case 'left':   return { x: G, y: (1 \- h) / 2, w, h };  
      case 'right':  return { x: 1 \- w \- G, y: (1 \- h) / 2, w, h };  
      case 'float': {  
        // float a bit off-center to feel deliberate, not sterile  
        const xJitter \= (Math.random() \- 0.5) \* 0.20;  
        const yJitter \= (Math.random() \- 0.5) \* 0.20;  
        const x \= Math.max(G, Math.min(1 \- w \- G, (1 \- w) / 2 \+ xJitter));  
        const y \= Math.max(G, Math.min(1 \- h \- G, (1 \- h) / 2 \+ yJitter));  
        return { x, y, w, h };  
      }  
    }  
  }

  // subdivide the leftover into three rectangles. the leftover region's  
  // shape depends on the anchor:  
  //   top    → one strip below  
  //   bottom → one strip above  
  //   left   → one strip on the right  
  //   right  → one strip on the left  
  //   float  → an annular region (a frame); we handle it by carving four  
  //            slabs (top, bottom, left, right of the audio cell) and  
  //            picking the three largest.  
  function subdivideLeftover(audioRect, anchor) {  
    const G \= 0.012;  
    if (anchor \=== 'top') {  
      return splitStripVertically(  
        { x: G, y: audioRect.y \+ audioRect.h \+ G,  
          w: 1 \- 2 \* G, h: 1 \- (audioRect.y \+ audioRect.h) \- 2 \* G }, 'h');  
    }  
    if (anchor \=== 'bottom') {  
      return splitStripVertically(  
        { x: G, y: G, w: 1 \- 2 \* G, h: audioRect.y \- 2 \* G }, 'h');  
    }  
    if (anchor \=== 'left') {  
      return splitStripVertically(  
        { x: audioRect.x \+ audioRect.w \+ G, y: G,  
          w: 1 \- (audioRect.x \+ audioRect.w) \- 2 \* G, h: 1 \- 2 \* G }, 'v');  
    }  
    if (anchor \=== 'right') {  
      return splitStripVertically(  
        { x: G, y: G, w: audioRect.x \- 2 \* G, h: 1 \- 2 \* G }, 'v');  
    }  
    // float: carve four slabs around the audio cell, pick the three  
    // largest by area, and use those as cells.  
    const slabs \= \[  
      { x: G, y: G,  
        w: 1 \- 2 \* G, h: audioRect.y \- G \* 2 },                                // top slab  
      { x: G, y: audioRect.y \+ audioRect.h \+ G,  
        w: 1 \- 2 \* G, h: 1 \- (audioRect.y \+ audioRect.h) \- 2 \* G },             // bottom slab  
      { x: G, y: audioRect.y,  
        w: audioRect.x \- G \* 2, h: audioRect.h },                               // left slab  
      { x: audioRect.x \+ audioRect.w \+ G, y: audioRect.y,  
        w: 1 \- (audioRect.x \+ audioRect.w) \- 2 \* G, h: audioRect.h }            // right slab  
    \].filter(s \=\> s.w \> 0.04 && s.h \> 0.04);  
    slabs.sort((a, b) \=\> (b.w \* b.h) \- (a.w \* a.h));  
    return slabs.slice(0, 3);  
  }

  // split a strip into three rects along its longest axis. cut points  
  // roll within \[0.22, 0.45\] of the strip length, so the three cells  
  // aren't equal but aren't lopsided either.  
  function splitStripVertically(strip, axis) {  
    // axis 'h' \= horizontal strip (cut along x), 'v' \= vertical strip (cut along y)  
    const rollCut \= () \=\> 0.22 \+ Math.random() \* 0.23;  
    if (axis \=== 'h') {  
      // cut along x. two cut points partition the width into three.  
      const t1 \= rollCut();  
      const t2Min \= t1 \+ 0.18;  
      const t2 \= t2Min \+ Math.random() \* Math.max(0.001, (1 \- t2Min \- 0.18));  
      const x1 \= strip.x;  
      const x2 \= strip.x \+ strip.w \* t1;  
      const x3 \= strip.x \+ strip.w \* t2;  
      const xEnd \= strip.x \+ strip.w;  
      const G \= 0.006;  
      return \[  
        { x: x1,       y: strip.y, w: x2 \- x1 \- G, h: strip.h },  
        { x: x2,       y: strip.y, w: x3 \- x2 \- G, h: strip.h },  
        { x: x3,       y: strip.y, w: xEnd \- x3,  h: strip.h }  
      \];  
    } else {  
      // cut along y. two cut points partition the height into three.  
      const t1 \= rollCut();  
      const t2Min \= t1 \+ 0.18;  
      const t2 \= t2Min \+ Math.random() \* Math.max(0.001, (1 \- t2Min \- 0.18));  
      const y1 \= strip.y;  
      const y2 \= strip.y \+ strip.h \* t1;  
      const y3 \= strip.y \+ strip.h \* t2;  
      const yEnd \= strip.y \+ strip.h;  
      const G \= 0.006;  
      return \[  
        { x: strip.x, y: y1, w: strip.w, h: y2 \- y1 \- G },  
        { x: strip.x, y: y2, w: strip.w, h: y3 \- y2 \- G },  
        { x: strip.x, y: y3, w: strip.w, h: yEnd \- y3 }  
      \];  
    }  
  }

  // ─── the engine ─────────────────────────────────────────────────────────  
  // input:  zone (string), recentAnchors (array)  
  // action: pick from zone pool → shape audio cell → subdivide leftover  
  // output: { zone, item, layout: { audio, others\[3\] }, embedHtml }  
  const recentAnchors \= \[\]; // memory window for alternation  
  const RECENT\_MAX \= 2;

  function sonicSort(zone) {  
    const pool \= \[  
      ...corpus.audio.filter(a \=\> a.zone \=== zone),  
      ...corpus.video.filter(v \=\> v.zone \=== zone)  
    \];  
    const item \= pool\[Math.floor(Math.random() \* pool.length)\];  
    const shaped \= shapeAudioCell(item.kind, recentAnchors);  
    const others \= subdivideLeftover(shaped.rect, shaped.anchor);

    // remember this anchor for next call  
    recentAnchors.push(shaped.anchor);  
    if (recentAnchors.length \> RECENT\_MAX) recentAnchors.shift();

    return {  
      zone, item,  
      layout: { audio: shaped.rect, anchor: shaped.anchor, others },  
      embedHtml: renderEmbed(item),  
      poolSize: pool.length  
    };  
  }

  function renderEmbed(item) {  
    if (item.kind \=== 'audio') {  
      return \`\<iframe  
        src="${item.embed}"  
        frameborder="0"  
        allow="autoplay; encrypted-media; fullscreen"  
        allowfullscreen  
        loading="lazy"  
        referrerpolicy="no-referrer-when-downgrade"\>\</iframe\>\`;  
    }  
    return \`\<video src="${item.src}" controls preload="metadata"\>\</video\>\`;  
  }

  // ─── render ─────────────────────────────────────────────────────────────  
  let last \= null;

  function pct(n) { return (n \* 100).toFixed(2) \+ '%'; }

  function renderRoom(result) {  
    const room \= document.getElementById('room');  
    room.innerHTML \= '';  
    if (\!result) {  
      // four placeholder cells in a default 2×2 just for the empty state  
      for (let i \= 0; i \< 4; i++) {  
        const cell \= document.createElement('div');  
        cell.className \= 'cell';  
        cell.style.left \= (i % 2 \=== 0 ? 0.5 : 50.5) \+ '%';  
        cell.style.top  \= (i \< 2 ? 0.5 : 50.5) \+ '%';  
        cell.style.width \= '49%';  
        cell.style.height \= '49%';  
        cell.innerHTML \= '\<span class="placeholder"\>—\</span\>';  
        room.appendChild(cell);  
      }  
      return;  
    }

    // audio cell  
    const audioCell \= document.createElement('div');  
    audioCell.className \= 'cell audio ' \+ result.zone;  
    Object.assign(audioCell.style, {  
      left: pct(result.layout.audio.x),  
      top:  pct(result.layout.audio.y),  
      width:  pct(result.layout.audio.w),  
      height: pct(result.layout.audio.h)  
    });  
    const num \= document.createElement('span');  
    num.className \= 'cell-num';  
    num.textContent \= '1';  
    audioCell.appendChild(num);  
    const wrap \= document.createElement('div');  
    wrap.style.width \= '100%';  
    wrap.style.height \= '100%';  
    wrap.innerHTML \= result.embedHtml;  
    audioCell.appendChild(wrap);  
    room.appendChild(audioCell);

    // three other cells (placeholders for now; will be filled by other engines)  
    result.layout.others.forEach((rect, i) \=\> {  
      const cell \= document.createElement('div');  
      cell.className \= 'cell';  
      Object.assign(cell.style, {  
        left: pct(rect.x),  
        top:  pct(rect.y),  
        width:  pct(rect.w),  
        height: pct(rect.h)  
      });  
      const n \= document.createElement('span');  
      n.className \= 'cell-num';  
      n.textContent \= (i \+ 2);  
      cell.appendChild(n);  
      const ph \= document.createElement('span');  
      ph.className \= 'placeholder';  
      ph.textContent \= '—';  
      cell.appendChild(ph);  
      room.appendChild(cell);  
    });  
  }

  function renderInfo(result) {  
    const info \= document.getElementById('info');  
    if (\!result) {  
      info.innerHTML \= '\<div class="empty"\>awaiting first call\</div\>';  
      return;  
    }  
    const it \= result.item;  
    const linkHtml \= it.kind \=== 'audio'  
      ? \`\<a href="${it.page}" target="\_blank" rel="noopener"\>${it.id}\</a\>\`  
      : \`\<a href="${it.src}" target="\_blank" rel="noopener"\>${it.id}\</a\>\`;  
    const dim \= \`${(result.layout.audio.w\*100).toFixed(0)}×${(result.layout.audio.h\*100).toFixed(0)}%\`;  
    info.innerHTML \= \`  
      \<div class="info-block"\>  
        \<div class="label"\>picked\</div\>  
        \<div class="val"\>${it.kind} · ${linkHtml}\</div\>  
      \</div\>  
      \<div class="info-block"\>  
        \<div class="label"\>anchor\</div\>  
        \<div class="val"\>${result.layout.anchor}\</div\>  
      \</div\>  
      \<div class="info-block"\>  
        \<div class="label"\>audio cell\</div\>  
        \<div class="val"\>${dim}\</div\>  
      \</div\>  
      \<div class="info-block"\>  
        \<div class="label"\>zone · pool\</div\>  
        \<div class="val"\>${result.zone} · ${result.poolSize}\</div\>  
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
    last \= sonicSort(zone);  
    renderCalled(last);  
    renderRoom(last);  
    renderInfo(last);  
    renderChipsActive(zone);  
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

  document.getElementById('callRandom').addEventListener('click', () \=\> {  
    call(rollZone());  
  });  
  document.getElementById('recall').addEventListener('click', () \=\> {  
    call(last ? last.zone : rollZone());  
  });

  // ─── init ───────────────────────────────────────────────────────────────  
  renderRoom(null);  
  renderInfo(null);  
\</script\>