# FuzzyFind · 0.1.0

Pure Buoy matching for an IDE's command palette, quick-open list or symbol picker.
It scores ordered subsequences, returns the best alignment's highlight positions,
and ranks candidates without changing their input order or contents.

Copy `packages/fuzzyfind-0.1.0.buipkg` into another project's `packages/` and
`Import fuzzyfind`. No native runtime or external dependency is required.

## API

| Call | Returns |
| --- | --- |
| `Match(queryText, candidateText, pathMode = False)` | A `FuzzyMatch`, including unsuccessful matches |
| `Rank(queryText, candidates, limit = 20, pathMode = False)` | Up to `limit` successful matches, highest score first |

`FuzzyMatch` exposes read-only `CandidateText`, `IsMatch`, `Score`,
`CandidateIndex` and `Positions`. In `Rank`, `CandidateIndex` is the original
zero-based input index, allowing callers to recover command IDs or other metadata.
`Match` uses `CandidateIndex = -1`. `Positions` returns an independent array of
zero-based **Unicode codepoint** indexes into the original candidate string.
They are neither UTF-8 byte offsets, UTF-16 units, nor grapheme-cluster indexes.

See [the runnable demo](../examples/fuzzyfind_demo.bui) for highlighted quick-open
results.

## Matching behavior

The query must appear in order, but may skip candidate characters. Matching uses
Unicode simple lowercase mappings, independent of locale. Original-case matches
receive a small bonus. There is no Unicode normalization or full case folding:
composed `é` and decomposed `e` + combining accent remain different sequences.

Scores reward contiguous characters and matches at the beginning, after common
word separators, or at ASCII lower-to-upper camel-case boundaries. Leading,
trailing and skipped characters are penalized. Dynamic programming chooses the
highest scoring alignment, including later contiguous matches that a greedy
first-match search would miss. Equal alignment scores select deterministically;
equal candidate scores preserve input order, including duplicate candidates.
Treat score magnitudes as implementation details, rather than confidence values.

With `pathMode = True`, characters in the final path component receive a bonus,
and `/` and `\` compare as equivalent separators. Candidate text and highlight
positions retain the original spelling. This is lexical path matching; it does
not touch the filesystem or resolve `.`/`..` components.

An empty query matches every candidate at score zero, so `Rank` returns the
first `limit` candidates. Unsuccessful `Match` results have `IsMatch = False`
and an empty position array; do not use their score for display or ranking.

## Work bounds

Each alignment takes O(query length × candidate length) time and predecessor
storage, with two rolling score rows. Limits throw `InvalidArgumentException`:

- Query: 128 codepoints; candidate: 4,096 codepoints.
- Alignment: 262,144 cells; ranking batch: 32,000,000 cells.
- Ranking input: 20,000 candidates; result limit: 0–500.

`Rank` validates candidate lengths and its total cell budget before scoring.
For larger workspaces, prefilter or partition candidates before ranking. A result
limit of zero returns immediately after validating the query and candidate count;
it does not inspect individual candidate strings. Inputs must be valid Buoy
strings. Embedded NUL behavior follows Buoy's string implementation.
