---

**Text Fragment Actuator**

Receives the called zone. Receives the written corpus.

The corpus here is the stories, the constitution, the transmissions, the writings about Mr. E and Brayne Snax and Paradoxic and Michael Mistree and QC. The Cryptic Syndicate body of text. Unlike the audio, video, and background pools, the corpus is not zone-tagged — text doesn't carry elemental belonging the way sound and image do. The corpus is one body. The zone influences what surfaces from it, but doesn't restrict it.

Three text presences appear per room. Three, because three of the four cells are text cells (the fourth is the audio cell from the Sonic Sorting System).

*What the engine may do.* Select an existing fragment from the corpus as-is. Recombine — take a sentence from one place and a sentence from another, place them adjacent without smoothing them together. Mutate — take a fragment and substitute a word, drop a clause, invert a phrase. Generate — produce something new, but only if the writing feels latent in the field already.

*What the engine must not do.* Summarize. Explain. Become motivational. Become generic mysticism. Smooth fragments into resolution. Add interpretive scaffolding. Conclude.

These constraints are load-bearing. The output isn't language about the corpus, it's language from the corpus. A fragment is allowed to be incomplete. It's allowed to not make sense in the room it appears in. The room will hold it.

*The mechanism.*

Step one: choose a mode for each of the three presences. The four modes are select, recombine, mutate, generate. Each presence rolls its own mode independently, weighted toward select and recombine (these honor the corpus most directly) and away from generate (which needs the most discipline to not become noise). Roughly: select 0.40, recombine 0.30, mutate 0.20, generate 0.10. These weights are dials.

Step two: for each mode, the source material is filtered by zone before sampling. The zone influence works through tonal proximity rather than tagging. Each fragment in the corpus has rough tonal characteristics — its imagery, its rhythm, its emotional weight. The zone biases sampling toward fragments whose tone resonates with that element. Water pulls toward flow, dissolution, depth, sorrow. Air pulls toward thought, distance, reflection, abstraction. Fire pulls toward action, defiance, intensity, transformation. Earth pulls toward grounding, body, history, matter. Meta pulls toward self-reference, paradox, the system noticing itself.

This bias does not need to be a tagging step done in advance. It can be a similarity score computed at sample time — for each fragment, a small set of keyword presences and rhythmic markers that lean it toward one zone or another. The score is soft; even a fragment scored low for the called zone has a non-zero chance of surfacing. The bias shapes probability, doesn't gate it.

Step three: produce three text presences, each one a string. Length varies — a presence can be a single word, a fragment of a sentence, a full sentence, a small cluster of two or three sentences. Length is part of the output, not predetermined. The Morphological Artifact Assigner will decide how to size and place these in their cells, but the engine emits whatever length the chosen fragment naturally has.

*Modes in detail.*

Select. Sample one fragment from the corpus, weighted by zone proximity. Emit it unchanged. The simplest mode and probably the most powerful.

Recombine. Sample two fragments from the corpus, both weighted by zone proximity. Place them adjacent — one then the other, separated by a line break or a single space, not smoothed. The juxtaposition is the work.

Mutate. Sample one fragment. Apply one mutation: substitute a single content word with another from the corpus's overall vocabulary; or drop a clause (cut at a comma or conjunction and keep one side); or invert a phrase (change "I am here" to "here I am"). One mutation, not many. The fragment should still feel like itself.

Generate. Sample two or three fragments, weighted by zone proximity, and use them as a tonal anchor for new language. This mode requires an LLM call or a careful template — without one, generate degrades into mysticism-noise, which is exactly what the spec forbids. If no LLM is available at runtime, generate falls back to recombine. If an LLM is available, the prompt instructs it: write one short fragment in the tonal register of these examples; do not summarize, do not explain, do not conclude; produce one to three sentences maximum; if the fragment doesn't feel latent in the examples, return nothing.

*Inputs.* Called zone, corpus, mode weights, recent-fragments memory (so the same fragment doesn't surface twice in a row).

*Outputs.* An array of three text presences, each one carrying its string content, its mode, and the source fragment IDs it drew from (for traceability).

---

**Morphological Artifact Assigner**

Receives the three text presences. Receives the three remaining cell rectangles from the Sonic Sorting System (the cells the audio doesn't occupy).

This engine doesn't decide what the text means. It decides how the text appears.

*Assignment.* The three presences need to land in the three cells. Assignment can be index-order (presence one in cell one, etc.) or it can be matched by length to cell area — long presences in larger cells, short presences in smaller cells, so type doesn't get squeezed or float in too much empty space. Length-to-area matching is probably right; it removes a layer of awkwardness.

*Per-cell decisions, for each presence in its cell.*

Size. The text's font size is a function of the cell's dimensions and the presence's character count. Roughly: target a fill ratio — the text should occupy maybe 60% to 80% of the cell's area when laid out. Compute a candidate size, check the resulting bounding box, adjust. A short fragment in a large cell uses bigger type; a long fragment in a small cell uses smaller type. Constrained within minimums and maximums (probably 11px floor, 64px ceiling) so nothing becomes unreadable or absurd.

Font. A small set, probably two or three. One serif for fragments that want gravity. One sans for fragments that want coolness. One monospace for fragments that want technicality or feel like transmissions. The choice can roll, weighted by mode — select-mode fragments tend toward serif (treating the corpus as something already written), generate-mode fragments tend toward sans or mono (newer, less settled). Or zone-influenced — fire wants energy in the type, water wants softness, air wants air. Or both.

Color. Three or four palette positions per zone. The zone provides a palette; within that palette, roll one color for this presence. Most colors stay near a luminance band that doesn't compete with the audio cell or the background field. Occasional outliers — a brighter color, a near-black against a light background — are allowed but rare.

Opacity. Within 0.75 to 1.0 for most presences; occasionally lower (0.55 to 0.75) for one of the three to create depth, so not all three text presences read at the same intensity.

Alignment. Left, right, center, or justified. Roll, weighted toward left and center (most legible). Right and justified are rarer, used when the cell shape suggests them — a tall narrow cell can take right alignment to feel architectural.

Rotation. Mostly zero. Occasionally a small rotation — five to fifteen degrees — for one presence per room, never more than one. This is a dial that should default off and only turn on rarely; over-rotation looks designed.

Spacing. Letter-spacing within a small range (-0.02em to 0.08em). Line-height adjusts to the type size, looser for larger sizes.

Emphasis. Per presence, decide whether one or two words within the fragment receive emphasis — a different weight, a different color, slight underline, italic. Rare. Most presences emit clean, no emphasis. About one in three or four presences gets an emphasis.

*The discipline.* Most rolls within most categories should land in the calm middle. The rare extremes are extremes precisely because they're rare. If every presence has rotation, color saturation, emphasis, and oddly-set type, the room becomes a poster. The spec says these things are tools; they're not all used at once.

*Inputs.* Three text presences (with their mode and source info), three cell rectangles, the called zone (for palette), recent-decisions memory (so the same color doesn't surface twice in a row, the same font doesn't dominate consecutive rooms).

*Outputs.* Three rendered text cells — each one's HTML/CSS or equivalent render directive, with the presence's content placed inside its cell at the chosen size, font, color, opacity, alignment, rotation, and spacing, with any emphasis applied to specific words.

---

The whole flow once these exist:

Shift sequence triggers. Navigator calls the next zone. Visualized Manifestation Placer crossfades the background field. Sonic Sorting System picks audio, shapes its cell, anchors it, subdivides the leftover into three cell rectangles. Text Fragment Actuator emits three text presences shaped by the zone. Morphological Artifact Assigner takes those three presences and the three cell rectangles and produces the rendered text cells. The room appears. Wait. Shift sequence triggers again.

---

That's the binary pair. The Text Fragment Actuator is the harder of the two to implement well, because the constraints on what it must not do are doing real work — it's easier to write code that summarizes than code that doesn't. The Morphological Artifact Assigner is more conventional, just careful about restraint.

---

**Text Fragment Actuator**

Receives the called zone. Receives the corpus.

Picks three fragments per room. Hands them off.

That's it.

The corpus is one body — the writings, the stories, the transmissions. The engine reads through it, chooses three fragments, passes them to the next engine. Zone influences which fragments surface — softly, through tonal proximity, not through tagging. A fragment can be a sentence, a phrase, a passage, whatever feels like a coherent piece.

Recent-fragments memory so the same fragments don't surface twice in a row.

Inputs: called zone, corpus, recent-fragments memory. Outputs: three fragments.

---

**Morphological Artifact Assigner**

Receives three fragments. Receives three cell rectangles from the Sonic Sorting System.

Places them in the cells. Sizes them so they fit. Makes the placement novel — the three fragments aren't styled identically; the typography varies between rooms.

Decisions made per presence:

* Which cell holds it (length to cell area is a reasonable match)  
* Font size that lets the fragment fill the cell appropriately  
* Font from a small set  
* Color from the zone's palette  
* Opacity  
* Alignment  
* Letter and line spacing

Recent-decisions memory so consecutive rooms don't look identical.

Inputs: three fragments, three cell rectangles, called zone, recent-decisions memory. Outputs: three rendered text cells.

---

