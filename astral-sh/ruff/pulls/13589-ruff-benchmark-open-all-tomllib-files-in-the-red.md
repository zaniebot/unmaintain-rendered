```yaml
number: 13589
title: "`ruff_benchmark`: open all `tomllib` files in the red-knot benchmark"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: redknot-bench-init
created_at: 2024-10-01T13:34:17Z
updated_at: 2024-10-01T16:47:38Z
url: https://github.com/astral-sh/ruff/pull/13589
synced_at: 2026-01-10T20:59:36Z
```

# `ruff_benchmark`: open all `tomllib` files in the red-knot benchmark

---

_Pull request opened by @AlexWaygood on 2024-10-01 13:34_

## Summary

I noticed that the red-knot benchmark was reporting different diagnostics on `tomllib` than red-knot did if you just ran red-knot from the CLI on the `tomllib` copied-and-pasted into an empty directory locally. Specifically, the following diagnostic was not being reported by the red-knot benchmark, but _was_ being reported if you just ran red-knot locally:

```
/src/tomllib/__init__.py:10:30: Name '__name__' used when not defined.
```

After some investigation, I realised that the culprit was this line here:

https://github.com/astral-sh/ruff/blob/2a36b47f1395284d32ae2015e6c626f242922224/crates/ruff_benchmark/benches/red_knot.rs#L76

"Opening" the `tomllib/_parser.py` file means that red-knot will only check `_parser.py` and files that `_parser.py` depends on. `_parser.py` doesn't depend on `__init__.py`, so `__init__.py` isn't being checked at all by the red-knot benchmark currently. This PR therefore changes the benchmark so that all `tomllib` files are "opened", and thus all `tomllib` files are checked.

The first commit in this PR should have no impact on behaviour: it's just some refactoring I did so that I could see more clearly which files were being downloaded by the benchmark and copied into the `tomllib` directory used by the benchmark. 

## Test Plan

`cargo bench -p ruff_benchmark -- red_knot`

---

_Label `red-knot` added by @AlexWaygood on 2024-10-01 13:34_

---

_Comment by @codspeed-hq[bot] on 2024-10-01 13:40_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/redknot-bench-init)

### Merging #13589 will **degrade performances by 28.18%**

<sub>Comparing <code>redknot-bench-init</code> (f5f8ae9) with <code>main</code> (2a36b47)</sub>



### Summary

`❌ 2 (👁 2)` regressions
`✅ 30` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `redknot-bench-init` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `red_knot_check_file[cold]` | 60.3 ms | 63.9 ms | -5.51% |
| 👁 | `red_knot_check_file[incremental]` | 3.1 ms | 4.3 ms | -28.18% |


---

_Renamed from "`ruff_benchmark`: open `__init__.py` and `_parser.py` in the red-knot benchmark" to "`ruff_benchmark`: open all `tomllib` files in the red-knot benchmark" by @AlexWaygood on 2024-10-01 13:44_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-01 13:52_

---

_Comment by @github-actions[bot] on 2024-10-01 14:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2024-10-01 14:49_

I'm okay with this change but What's the motivation for checking all files? Or why is it important for the benchmark to check all files?

---

_Comment by @AlexWaygood on 2024-10-01 14:56_

> What's the motivation for checking all files? Or why is it important for the benchmark to check all files?

Two reasons I guess, but neither of them are directly related to the benchmark's function "as a benchmark":
1. I was trying to repro the issues Charlie was seeing in https://github.com/astral-sh/ruff/pull/13579#issuecomment-2384751176, and it was just very confusing to me that I was seeing different diagnostics when running red-knot on `tomllib` directly than the ones that we were asserting in the benchmark. It's much easier to understand what's going on if the benchmark reports the same diagnostics that you get when executing red-knot on the directory from the CLI.
2. Probably we should add a separate test for this that just runs red-knot on `tomllib` as part of our test suite -- but this benchmark, as well as measuring performance, also serves as a really useful integration test that shows the kinds of false positives we currently emit on well-written real-world code. I sort-of think about the diagnostic list in the benchmark file as something of a todo list.

Also -- and I suppose this _is_ more related to its function as a benchmark -- I assumed that the benchmark _was_ indicative of "the time it takes to check the `tomllib` directory". I suppose it doesn't really matter so much if that's _not_ what it's indicative of -- but it does feel, again, slightly less confusing to me if it checks the whole directory.

---

_@carljm approved on 2024-10-01 16:45_

This seems fine to me; I don't think it matters a lot either way for the benchmark, and I agree this is less confusing when looking at the error output.

---

_Merged by @AlexWaygood on 2024-10-01 16:47_

---

_Closed by @AlexWaygood on 2024-10-01 16:47_

---

_Branch deleted on 2024-10-01 16:47_

---
