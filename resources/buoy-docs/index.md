---
title: "FuzzyFind"
summary: "Rank ordered-subsequence matches and recover Unicode highlight positions."
category: "Overview"
order: 0
keywords: ["fuzzy", "search", "palette", "quick-open", "ranking"]
related: ["getting-started.md", "api.md", "typed-api.md"]
---

# FuzzyFind

FuzzyFind is a pure Buoy matcher for command palettes, quick-open lists, and symbol pickers. It finds the highest-scoring ordered-subsequence alignment and returns matching positions, while preserving candidate spelling and input metadata indexes. It does not read the filesystem and has no external dependency.

## Match semantics

The query's characters must appear in order, but may skip candidate characters. Matching uses Unicode simple lowercase mapping with a small original-case bonus. There is no locale-dependent matching, Unicode normalization, or full case folding. Scores reward contiguity, separators, starts, and ASCII camel-case boundaries; treat score magnitudes as implementation details.

In path mode, slash and backslash match as separators, and the final component receives a bonus. This is lexical matching only, without resolving paths or dot components.

## Highlight coordinates

`Positions` contains zero-based Unicode codepoint indexes into the original candidate. They are neither UTF-8 bytes nor UTF-16 units nor grapheme-cluster indexes. Use `String.Chars` when mapping these positions to characters. Results return defensive copies of position arrays.

## Read next

- [Getting started](getting-started.md)
- [API reference](api.md)
