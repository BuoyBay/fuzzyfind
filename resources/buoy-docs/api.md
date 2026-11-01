---
title: "FuzzyFind API"
summary: "Public types and operations supplied by FuzzyFind."
category: "Reference"
order: 20
keywords: ["fuzzy", "search", "palette", "quick-open", "ranking"]
symbols: ["FuzzyMatch", "Match", "Rank", "FuzzyMatch.Positions", "FuzzyMatch.CandidateIndex", "FuzzyMatch.Score", "FuzzyMatch.IsMatch"]
related: ["index.md", "getting-started.md", "typed-api.md"]
---

# API reference

## Calls

| Symbol | Contract |
| --- | --- |
| `Match(queryText, candidateText, pathMode = False)` | Return a `FuzzyMatch` even when unsuccessful. |
| `Rank(queryText, candidates, limit = 20, pathMode = False)` | Return at most `limit` successful matches, highest score first. |

Each alignment uses dynamic programming in query-length times candidate-length time and predecessor storage, with two rolling score rows. The best alignment can start later than a greedy first match. Tie resolution is deterministic.

## Result

| Property | Meaning |
| --- | --- |
| `CandidateText` | Original candidate spelling. |
| `IsMatch` | Whether the full query matched in order. |
| `Score` | Relative ranking value; do not display it as a probability. |
| `CandidateIndex` | Original zero-based index for Rank; -1 for Match. |
| `Positions` | Independent array of codepoint indexes; empty on failure. |

No candidate list is modified. Inputs must be valid Buoy strings; embedded NUL behavior follows the language runtime. Rendering highlights, handling keyboard navigation, and retaining command IDs belong to the caller.

[Getting started](getting-started.md) · [Package overview](index.md)


[Full typed API: properties, parameters, defaults, and returns](typed-api.md)
