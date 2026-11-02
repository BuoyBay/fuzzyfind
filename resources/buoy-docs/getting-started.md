---
title: "FuzzyFind getting started"
summary: "Install FuzzyFind and try a complete usage example."
category: "Guides"
order: 10
keywords: ["fuzzy", "search", "palette", "quick-open", "ranking"]
related: ["index.md", "api.md", "typed-api.md"]
---

# Getting started

Copy `fuzzyfind-0.1.0.buipkg` into `packages/` and import `fuzzyfind`.

## Rank quick-open candidates

Keep metadata in a parallel array or map. `CandidateIndex` refers to the original candidate list even after results have been sorted.

```basic
Import fuzzyfind

Sub Main()
    Dim candidates As Array Of String = ["src/main.bui", "tests/main_test.bui", "docs/manual.md"]
    For Each matched As FuzzyMatch In Rank("mn", candidates, 3, True)
        Print String.From(matched.CandidateIndex) + ": " + matched.CandidateText
        Dim characters As Array Of String = String.Chars(matched.CandidateText)
        For Each position As Integer In matched.Positions
            Print "Matched: " + characters(position)
        Next
    Next
End Sub
```

## Bound the candidate set

Queries are limited to 128 codepoints, each candidate to 4096 codepoints, and one alignment to 262,144 cells. Ranking accepts up to 20,000 candidates, a total 32,000,000-cell budget, and result limits from 0 through 500. Exceeded bounds throw `InvalidArgumentException`; prefilter or partition large workspaces before scoring.

An empty query matches every candidate at score zero and keeps input order. Equal candidate scores also keep input order, including duplicates. An unsuccessful `Match` has `IsMatch = False` and no positions; ignore its score. Limit zero returns after validating query/candidate count without scanning candidate strings.

[Package overview](index.md) · [API reference](api.md)
