# Explainer patterns

Read the pattern matching the process being explained. Combine patterns only when the underlying system genuinely combines them.

## Linear token pipeline

Use for sequential transformations where one output becomes the next input.

```text
Input token → Parse → Normalize → Classify → Result
```

Keep all stages visible. At each step, move the same token forward and update its payload. The detail panel should show before and after values, not repeat the stage name.

## Branch and merge

Use when inputs split into multiple paths or independent signals combine.

```text
Query ──→ semantic path ──┐
                          ├─→ merge/rank → result
Document → metadata path ─┘
```

Give tokens stable identities, colors, or shapes paired with labels. Animate each path independently. At the join, visibly collect every required input before activating the merge node. Do not imply a merge if one branch merely wins and the others are discarded.

## Concurrent windows or workers

Use aligned lanes rather than serial animation.

```text
Window 1 → Choice/Noul → candidates
Window 2 → Choice/Noul → candidates
Window 3 → Choice/Noul → candidates
                         ↓
                  shared verification
```

Start lanes together, show per-lane outputs, then join them at the shared stage. Distinguish concurrency from simultaneity claims: an implementation may use a bounded worker pool rather than starting every job at once. Show the worker limit when it affects understanding.

## Persistent-frontier search

Use for best-first search, verification-guided backtracking, or recoverable routing.

Keep three visible regions:

1. The stable document/search tree.
2. The active path.
3. The persistent frontier or queue of unvisited alternatives.

The critical failure sequence is:

```text
select best branch
→ search it deeply
→ verification fails
→ mark branch closed
→ promote next retained branch
→ continue
```

Animate the failed path into a closed state and the next frontier item into active state. Do not erase low-ranked paths if the algorithm retains them.

For partial evidence, add an evidence ledger and update the remaining information need before choosing the next branch.

## Beam search

Use parallel active paths by depth. Show the beam width and the pruning boundary.

```text
rank expansions → keep top K → permanently prune remainder → expand survivors
```

Make permanent pruning visually different from a persistent frontier. If pruned paths remain recoverable, the algorithm is not conventional beam search; describe it as a frontier or archive.

## Relative choice followed by absolute verification

Show the semantic difference between stages:

```text
Choice: Which option is best relative to this menu?
Noul/verification: Does this candidate independently satisfy the predicate?
```

Choice probabilities from separate menus are local. Keep per-menu candidates first, then place them into a shared verification stage. Include a `None` option as diagnostic only if that matches the implementation; do not claim it proves absence.

## Code-linked trace

For each visible stage, associate the smallest responsible unit:

```text
Stage label
Function: search_pages(...)
Input: section ranges + page text
Output: candidate page IDs
Source: tree_search.py:108
```

Reveal code on selection or in the current-step panel. Keep the graph readable without the code panel.

## Suggested step-state shape

Use a data-driven state model so controls update one consistent visual:

```js
const steps = [
  {
    heading: "Rank root sections",
    focus: ["root", "presidency"],
    input: "Question + unvisited section labels",
    operation: "Relative Choice",
    output: "Presidency becomes active",
    edges: { "root-presidency": "active" },
    frontier: [
      ["Presidency", "active"],
      ["Biography", "waiting"]
    ],
    source: "search_sections()"
  }
];
```

Render from state rather than scattering step-specific DOM mutations across event handlers. This keeps Previous, timeline jumps, resize, and reduced-motion behavior consistent.
