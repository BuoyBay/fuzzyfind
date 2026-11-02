# Unicode subsequence ranking for quick-open

Source-only matching for an IDE's command palette, quick-open list or symbol picker.
It scores ordered subsequences, returns the best alignment's highlight positions,
and ranks candidates without changing their input order or contents.

Copy `packages/fuzzyfind-0.1.0.buipkg` into another project's `packages/` and
`Import fuzzyfind`. No native runtime or external dependency is required.

## Install

From a Buoy consumer project with BPM installed:

```sh
bpm add fuzzyfind --git https://github.com/BuoyBay/fuzzyfind.git --rev refs/heads/main
```

BPM records the resolved commit in its lockfile. Keep that lockfile with the
consumer project for reproducible installs.

## Example

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

## Build from source

With the native `build-packages` driver from [devtools](https://github.com/BuoyBay/devtools)
on your `PATH`, run from this checkout:

```sh
build-packages --root "$PWD" --compile-consumer fuzzyfind
```

The driver checks source targets and compiles an import consumer. See the
[API guide](docs/API.md) for behavior, limits and any host prerequisites.

## Local Buoy setup

On your own machine, install Buoy and run:

```sh
buoy ai init --both
```


## Contributing

[Contributing](CONTRIBUTING.md) · [License](LICENSE)
