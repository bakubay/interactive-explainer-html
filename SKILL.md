---
name: interactive-explainer-html
description: Create or improve standalone interactive HTML explainers that let a reader follow data, queries, decisions, branches, merges, queues, or evidence through a system step by step. Use for execution traces, algorithm walkthroughs, dataflow diagrams, retrieval/search pipelines, and code-linked process explanations; do not use for ordinary marketing sites or static prose documentation.
---

# Interactive Explainer HTML

Build a direct, standalone HTML document that makes an invisible process spatially and temporally understandable. The reader should be able to answer, at every step: what entered, what acted on it, what came out, where it goes next, and why.

## Establish the trace

Inspect the relevant code, documentation, or user-supplied process before designing the visual. Separate verified behavior from illustrative example data. Never present invented probabilities, page ranges, timings, or code paths as observed facts; label them as illustrative when used to teach the mechanism.

Model these elements explicitly:

- **Tokens:** the query, document, page, candidate, score, evidence, or other payload that moves.
- **Nodes:** the functions, services, decisions, queues, or stages that transform or route tokens.
- **Edges:** where each output goes, including branches, retries, backtracking, and merges.
- **State:** waiting, active, verified, rejected, closed, merged, or completed.
- **Provenance:** which source file, function, or line range is responsible when code is available.

Write a compact step table before implementation: `focus`, `input`, `operation`, `output`, `next destination`, `state changes`, and `source`. This is working material, not necessarily visible in the final page.

For branching, concurrency, multiple inputs, merges, beam/frontier search, or backtracking, read [references/explainer-patterns.md](references/explainer-patterns.md).

## Interaction model

Keep one stable graph or pipeline on screen. Change its state as the reader advances instead of replacing it with unrelated slides.

Provide:

- Previous, Next, and Play/Pause controls.
- A clickable step timeline or progress rail.
- Keyboard navigation when appropriate: left/right arrows and space for play/pause.
- A concise current-step panel showing **Input → Operation → Output**.
- Visible state for queues, frontiers, closed branches, accumulated evidence, or merged inputs when those concepts matter.
- A useful first frame before any interaction.

The focused token or branch should move forward through the existing graph. Dim context without hiding information the reader needs to understand alternatives. If multiple tokens follow different paths, give them stable identities and show the join explicitly.

Do not make a user hover to discover essential information.

## Visual and motion language

Use a restrained technical-editorial design: strong hierarchy, readable labels, a limited semantic palette, and enough whitespace to follow the trace. Color must be paired with text or shape labels.

Keep semantic states stable:

- Active or currently executing.
- Waiting or retained for later.
- Rejected or exhausted.
- Verified or successful.
- Neutral structure.

Animate only to explain causality, spatial continuity, or state change. Prefer CSS transitions and Web Animations using `transform` and `opacity`. Keep ordinary UI feedback around 150–300ms; explanatory token travel may be slower when it helps comprehension. Avoid decorative looping motion, except a subtle active-edge or scan indicator. Support `prefers-reduced-motion` with a readable non-moving state.

## Content discipline

Use realistic example inputs. Preserve exact payloads when they are short; summarize only when the full value would overwhelm the graph, and allow the important portion to remain visible.

Explain the mechanism, not merely the labels:

- Show why a branch was selected.
- Show what was retained versus discarded.
- Show whether a score is local, global, relative, or independently verified.
- Show stopping conditions and failure recovery.
- Show the actual merge or backtrack rather than mentioning it in prose.

When connecting stages to source code, show the smallest responsible unit or a precise file/function reference. Do not dump whole files into the page.

## Deliverable

Unless the user explicitly requests a component or project integration, produce one complete standalone `.html` file with embedded CSS and JavaScript:

- No iframe wrapper around another artifact.
- No build step or local server requirement.
- No external dependency unless it materially improves the explanation and is safe for a local file.
- Responsive from narrow mobile widths through desktop.
- Semantic controls, visible focus states, sufficient contrast, and live-region updates for changing step descriptions.
- Directly openable with `file://`.

Honor the user's requested path. Otherwise save it in the task's user-facing output directory. Open the finished file in the available preview when helpful.

## Validation

Before delivery:

1. Validate HTML parsing and JavaScript syntax.
2. Check that every queried element and every step target exists.
3. Exercise Previous, Next, Play/Pause, timeline selection, and the final state.
4. Check the initial, branching/failure, merge/backtrack, and completion states.
5. Check narrow width and reduced-motion behavior.
6. Confirm that source claims match the inspected code and illustrative values are labeled.

## Avoid

- A decorative flowchart with no visible intermediate payloads.
- A vertical stack of cards pretending to be a dataflow graph.
- Replacing the entire page at each step, which destroys spatial memory.
- Hard-pruning branches in the visual when the algorithm retains them.
- Globally comparing probabilities that are only local to separate decisions.
- Dense permanent prose panels, redundant legends, or dashboard metrics that do not explain the trace.
- Tiny labels, color-only state, hover-only details, uncontrolled auto-play, or motion without reduced-motion support.
