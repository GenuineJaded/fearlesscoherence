# Fearless Coherence Room Source Map

This is a handle, not a replacement for the room.

## Core Files

- `index.html` is the runtime surface. The room, sanctuary page, media lists, engines, render loop, and controls all live here.
- `corpus.js` is the text body. It exports `CORPUS`, currently 1,596 fragments from the Cryptic Syndicate volumes.
- `ORIENTATION.md` is conceptual architecture. It should shape interpretation, not be flattened into feature requirements.
- `compost-session-01.md`, `compost-session-02.md`, and `compost-session-03.md` are handoff soil from prior sessions.
- `render.yaml` confirms this is deployed as a static site from the repo root.

## Page Shape

- The randomized room is the home page.
- The static page is `#sanctuary` inside `index.html`.
- `SANCTUARY` opens the static grounding page.
- `Return` closes sanctuary and renders a new room.

## Current Engines

1. `Paradox Zone Navigator`
   - Code: `callZone()`
   - Chooses one of `water`, `air`, `fire`, `earth`, `meta`.
   - Keeps short memory so the same recent zones do not repeat too tightly.

2. `Visualized Manifestation Placer`
   - Code: `placeBackground(zone)`
   - Picks a background from the current zone and fades it into the room.

3. `Tonal Trinity Tradition`
   - Code: `tonalTrinityTradition(zone)`, `subdivide()`, `splitStrip()`
   - Picks audio or video from the current zone.
   - Places the media cell.
   - Creates leftover pockets where text can appear.

4. `Text Fragment Actuator`
   - Code: `sliceGlyph()`, `pickGlyph()`, `actuateFragments()`
   - Pulls three text glyphs from the whole corpus.
   - Current behavior is random word-window slicing.
   - This is the first place to work if the fragments feel weak, noisy, or undercooked.

5. `Coherence Clearance Crystallizer`
   - Code: `coherenceClearanceCrystallizer()`, `chooseCrystallizedCell()`
   - Receives the called zone, background, chosen transmission, fragments, and open cells.
   - Clears room pressure into three usable cell opportunities.
   - Keeps whole-room balance out of the artifact assigner.

6. `Morphological Artifact Assigner`
   - Code: `targetTactile()`
   - Receives the three glyphs and crystallized cell opportunities.
   - Controls font, size, color, opacity, alignment, line height, letter spacing, and occasional rotation.

## Text Fragment Work Starts Here

The current text issue is not primarily in `corpus.js`.

It starts in `index.html`:

- `sliceGlyph()` currently cuts arbitrary word windows.
- `pickGlyph()` chooses random corpus entries and avoids recent repeats.
- `actuateFragments()` returns three glyphs per room.
- `coherenceClearanceCrystallizer()` now handles room balance before the glyphs are rendered.

Likely next refinement:

- Keep the whole corpus available.
- Improve how slices begin and end.
- Favor fragments that feel like utterances rather than accidental clippings.
- Avoid turning this into rigid filtering or zone-gating; prior sessions established that text lives outside the five-zone system.

## Open Threads

- Text fragments need more felt coherence without losing fractal randomness.
- Mobile layout may let text/media overlap corner controls; fix through room awareness rather than hard fences.
- Audio filenames should be periodically verified against CDN reality if tracks fail.
- `Return` may want stronger button presence.

## Boundary

The source of truth is distributed:

- mechanical truth lives in `index.html` and `corpus.js`
- conceptual truth lives in `ORIENTATION.md` and the named design docs
- session continuity lives in the compost files

Do not collapse these into one document.
