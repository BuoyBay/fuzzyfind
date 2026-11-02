---
title: "fuzzyfind typed API"
summary: "Source-derived public signatures, properties, parameters, defaults, and return values."
category: "Reference"
order: 21
types: ["FuzzyMatch"]
dependencies: []
symbols: ["FuzzyMatch"]
related: ["api.md", "getting-started.md", "index.md"]
---

# fuzzyfind typed API

Generated from the package source declarations. Platform guards are shown where present; inherited members are documented on the base type. See the [API contracts](api.md) for behavior and limitations.

## FuzzyMatch

`Public Class FuzzyMatch`

An immutable match result. CandidateIndex refers to the input array in Rank.

### Properties

| Name | Type | Read-Only | Shared | Default | Description |
| --- | --- | --- | --- | --- | --- |
| `CandidateText` | `String` | Yes | No | — | — |
| `IsMatch` | `Boolean` | Yes | No | — | — |
| `Score` | `Integer` | Yes | No | — | — |
| `CandidateIndex` | `Integer` | Yes | No | — | — |
| `Positions` | `Array Of Integer` | Yes | No | — | — |

### Match

`Public Function Match(queryText As String, candidateText As String, pathMode As Boolean = False) As FuzzyMatch`

Find the best ordered subsequence alignment; unmatched input returns IsMatch=False.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `queryText` | `String` | `ByVal` | Required |
| `candidateText` | `String` | `ByVal` | Required |
| `pathMode` | `Boolean` | `ByVal` | `False` |

#### Returns

| Type |
| --- |
| `FuzzyMatch` |

### Rank

`Public Function Rank(queryText As String, candidates As Array Of String, limit As Integer = 20, pathMode As Boolean = False) As Array Of FuzzyMatch`

Return highest scoring candidates. Equal scores retain input order; no input is mutated.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `queryText` | `String` | `ByVal` | Required |
| `candidates` | `Array Of String` | `ByVal` | Required |
| `limit` | `Integer` | `ByVal` | `20` |
| `pathMode` | `Boolean` | `ByVal` | `False` |

#### Returns

| Type |
| --- |
| `Array Of FuzzyMatch` |
