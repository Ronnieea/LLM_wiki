# Skill: Sleep Science Wiki → HTML Teaching Slides (Loop)

## Goal
Use the existing `The Science of Sleep` LLM Wiki to produce a classroom-ready, browser-based HTML teaching slide deck. This is an iterative **Loop**, not a one-shot prompt or a Graph. Deliver 8–15 slides (target 10–12) and a verifiable execution log.

## Trigger
Run when the user requests an HTML teaching deck based on the completed sleep-science Wiki. Start only after the preflight gate passes. Do not silently replace the Wiki with general model knowledge.

## Inputs and authority
- Read existing `AGENTS.md` first and obey it. If conflicts exist, report them before proceeding.
- Read `wiki/index.md` and linked Wiki Markdown pages, `log.md`, and the actual bibliography/source mapping available in the project.
- The Wiki is the **sole scientific content source** for slide claims. The original PDFs in `raw/` may be consulted **only to audit or resolve Wiki citations**, not to silently introduce claims absent from the Wiki.
- If the Wiki contains fewer than five successfully ingested, verifiable papers, or if essential topic coverage is missing, stop and report the gap; do not invent material.

## Constraints
1. Preserve `raw/`, existing Wiki pages, and scientific conclusions. No overwriting source files.
2. Use Traditional Chinese for teaching prose; keep original English research titles and technical terms where useful.
3. Audience: general university students. Explain jargon on first use.
4. Deck: 8–15 slides, target 10–12; cover learning goals, sleep stages, circadian rhythm, sleep/memory, sleep deprivation/cognition, caffeine/sleep, practical implications, limitations, summary, references **only where supported by Wiki**.
5. Every scientific claim, numerical result, and figure must have a traceable Wiki citation. Cite paper/source ID and Wiki path; when possible, section/page/table/figure. Do not invent missing precision.
6. Avoid false certainty and extrapolation; distinguish primary studies from reviews and association from causation.
7. HTML/CSS/JS must run locally without requiring a network or external CDN; responsive 16:9 projector layout, legible fonts, keyboard left/right navigation, clickable previous/next buttons, slide counter. Provide a references slide.
8. No extra features (quiz, animations, external assets) unless they do not compromise accuracy, accessibility or schedule.
9. Do not mark checks PASS without executing them. Missing browser testing is NOT VERIFIED, not PASS.
10. Keep outputs in `presentation/` and run logs in `presentation/loop_runs/`; preserve prior run reports.

## Five-part loop contract
- **Goal:** evidence-grounded HTML teaching deck.
- **Constraints:** as above.
- **Completion criteria:** all mandatory automated checks PASS, independent content review PASS, visual browser review PASS; evidence and coverage complete.
- **Stop condition:** PASS all gates; OR at most 3 correction rounds after the initial draft; OR blocked by missing evidence/tools, unresolved contradiction, or serious factual issue. On stop with failure, report FAIL/BLOCKED and do not claim completion.
- **Trigger:** user explicitly asks to generate the deck after Wiki ingestion.

## Execution sequence
### Preflight (once)
- Inventory Wiki pages, source IDs, paper metadata, and actual ingestion evidence. Record which five papers are verified.
- Verify Wiki references and required topic coverage. Create `presentation/loop_runs/<run-id>/preflight.md`.
- If the preflight fails, stop. Do not repair Wiki content without user approval.

### Initial draft (iteration 0)
1. Extract evidence cards from Wiki, each with: `claim_id`, claim, qualification, source_id, wiki_path, source_locator, supporting excerpt or exact section, proposed slide. Store `evidence.json`.
2. Draft an outline mapping each slide to evidence cards; store `outline.md`.
3. Generate structured slide content and `presentation/index.html`, plus local CSS/JS if needed. Keep slide claims traceable in `presentation/slide_sources.json`.
4. Run the validation gates below. Write `iteration-0.json` and `iteration-0.md` with individual PASS/FAIL/NOT VERIFIED, exact command or procedure, and findings.

### Validation gates (each iteration)
**A. Automated structural tests (required):**
- 8–15 actual slides.
- Each scientific slide/claim has a valid Wiki source ID and existing Wiki path; all slide source references resolve.
- Every displayed number and scientific assertion is covered by the evidence cards (flag for human/AI review when semantic checking is required).
- Local assets, links and references resolve; no network-only dependency.
- Previous/next buttons, keyboard navigation, slide counter exist and function under actual browser testing (static existence alone is insufficient).
- HTML parses and opens without JS console errors where browser tooling is available.
- Produce a reproducible validation script if practical; include commands and outputs.

**B. Independent content review (required):**
- Use an independent reviewer context/subagent **if available**, supplied only with the finished slides, cited Wiki excerpts, and the source mapping, not the generation rationale. Review factual fidelity, coverage, study limitations, unsupported numbers, and misleading simplifications.
- If independent agent context is unavailable, create `review_packet.md` and mark `INDEPENDENT_REVIEW_PENDING`; do not claim it passed. A fresh external chat can review this packet.

**C. Visual/browser review (required):**
- Open slides at desktop projector 16:9 and a narrower viewport; check clipping, overlap, readability, missing fonts/graphics, controls and slide changes. Save screenshots or a concise per-slide visual log.
- If browser tools are unavailable, mark `VISUAL_REVIEW_PENDING` and provide exact manual steps; do not claim visual PASS.

### Correction loop (max 3 rounds)
- If any gate fails, build a prioritized defect list with file, slide, evidence, reproduction, and severity.
- Fix **only** issues in that report. Do not rewrite unrelated slides, introduce new scientific claims, or silently modify Wiki.
- Re-run **all** applicable checks and log iteration 1, 2, or 3.
- Exit immediately on all-pass; after round 3 stop even if FAIL remains.

## Output contract
- `presentation/index.html` (and local CSS/JS assets if needed)
- `presentation/slide_sources.json`
- `presentation/loop_runs/<run-id>/evidence.json`
- `presentation/loop_runs/<run-id>/outline.md`
- `presentation/loop_runs/<run-id>/iteration-0.md` and any `iteration-1..3.md`
- `presentation/loop_runs/<run-id>/review_packet.md` if independent review not available
- `presentation/loop_runs/<run-id>/final_report.md` with outcome `PASS`, `FAIL`, or `BLOCKED`, checks, attempts, unresolved issues, and how to open deck
- Append a concise summary and references to these reports in the project's `log.md`.

## Reviewer rubric
1. Scientific fidelity: every claim matches cited Wiki evidence, including conditions and uncertainty.
2. Coverage: central concepts in the Wiki are taught in coherent order; no invented coverage.
3. Pedagogy: definitions and transitions are understandable to university non-specialists.
4. Visual: no clipping, tiny fonts, or overwhelming text; citations readable.
5. Function: navigation and source links actually work.

## Hard prohibitions
- Do not claim that merely generating a report means the tests were run.
- Do not use external internet knowledge to fill Wiki gaps.
- Do not fake a separate AI review, screenshots, Git commits, or test results.
- Do not exceed three correction rounds.
