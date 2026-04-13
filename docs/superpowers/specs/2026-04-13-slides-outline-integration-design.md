# Slides Outline Integration Design

## Goal

Integrate the structure from `slides_outline.md` into `slides.tex` by replacing the placeholder Beamer deck with a clean scaffold that mirrors the outline hierarchy. The result should compile as a usable presentation skeleton without inventing substantive slide content.

## Scope

This change updates `slides.tex` only. `slides_outline.md` remains the source outline and is not modified.

## Source Material

- `slides_outline.md` contains:
  - presentation title
  - author list
  - a heading hierarchy using `##`, `###`, and `####`
- `slides.tex` currently contains:
  - placeholder title and author metadata
  - a Celestia Beamer theme setup
  - a title page
  - an outline slide
  - placeholder sections and frames

## Mapping Rules

The integration uses the following deterministic mapping:

- Markdown document title becomes Beamer `\title`
- Markdown author line becomes Beamer `\author`
- Existing Beamer theme setup is preserved
- Existing title page and outline slide are preserved
- `##` headings map to Beamer `\section`
- `###` headings map to Beamer `\subsection`
- `####` headings map to placeholder frames

## Placeholder Frame Rules

- Every leaf topic should have a concrete frame in the output deck
- A `####` heading is always treated as a leaf topic and becomes a frame titled with that heading text
- The frame body contains a minimal placeholder marker such as `\textit{To be developed}`
- If a `##` section has no `####` descendants anywhere beneath it, add one placeholder frame for that section so the outline item appears as an actual slide
- If a `###` subsection has no `####` descendants beneath it, add one placeholder frame for that subsection
- Higher-level headings that already contain lower-level leaf topics remain structural navigation nodes only and do not get duplicate frames

## Expected Deck Shape

The resulting deck should have:

1. title page
2. outline slide
3. one or more content frames for each top-level section in the outline
4. one placeholder frame for each terminal topic

For the current outline, this means:

- `Sequence Space v.s State Space` becomes a section with its own placeholder frame because it has no lower-level children
- `Local solution` becomes a section
- `SSJ` and `SSJ development` become subsections under `Local solution`
- the `####` items under `SSJ development` become frames
- `Application` becomes a subsection under `Local solution`
- the `####` items under `Application` become frames
- `Global Solution` becomes a section
- its `###` items become subsection-level placeholder frames because they have no `####` children
- `Reference` becomes a section with its own placeholder frame because it has no lower-level children

## Non-Goals

- drafting substantive bullet content for slides
- changing the visual theme
- editing `slides_outline.md`
- adding automation to parse Markdown into LaTeX

## Verification

After editing `slides.tex`, verify by compiling the deck if a LaTeX engine is available in the environment. If compilation is not possible, report that limitation explicitly.
