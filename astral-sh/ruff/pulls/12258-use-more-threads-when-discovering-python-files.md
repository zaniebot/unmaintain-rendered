```yaml
number: 12258
title: Use more threads when discovering python files
type: pull_request
state: merged
author: MichaReiser
labels:
  - performance
assignees: []
merged: true
base: main
head: reduce-lock-congestion
created_at: 2024-07-09T12:55:13Z
updated_at: 2024-07-10T07:29:18Z
url: https://github.com/astral-sh/ruff/pull/12258
synced_at: 2026-01-12T15:55:40Z
```

# Use more threads when discovering python files

---

_@MichaReiser_

## Summary

The `ignore` crates defaults to two threads when using `build_parallel`. This PR adopts a simplified heuristic from ripgrep to get a "good" number of threads to use. This gives me on my machine a 25% boost when checking an already cached project.

I also refactored the visitor to use a custom visitor builder. 
That allows us to push to local `Vec` and only merging them when the visitor gets dropped. 
I hoped for a more significant speedup than 0-1%. I don't mind reverting that change. 

## Test Plan

* `ruff-main`: Baseline of this PR
* `ruff`: This PR
* `ruff-fn`: This PR but without using a custom visitor builder

```
❯ hyperfine --warmup 10 \
        "./target/release/ruff-main check ./crates/ruff_linter/resources/test/cpython/ -e -s" \
        "./target/release/ruff check ./crates/ruff_linter/resources/test/cpython/ -e -s" \
       "./target/release/ruff-fn check ./crates/ruff_linter/resources/test/cpython/ -e -s"
Benchmark 1: ./target/release/ruff-main check ./crates/ruff_linter/resources/test/cpython/ -e -s
  Time (mean ± σ):      26.2 ms ±   1.1 ms    [User: 29.2 ms, System: 43.9 ms]
  Range (min … max):    23.5 ms …  28.4 ms    109 runs
 
Benchmark 2: ./target/release/ruff check ./crates/ruff_linter/resources/test/cpython/ -e -s
  Time (mean ± σ):      21.2 ms ±   1.0 ms    [User: 37.7 ms, System: 58.7 ms]
  Range (min … max):    18.4 ms …  24.6 ms    130 runs
 
Benchmark 3: ./target/release/ruff-fn check ./crates/ruff_linter/resources/test/cpython/ -e -s
  Time (mean ± σ):      21.4 ms ±   0.9 ms    [User: 38.3 ms, System: 58.3 ms]
  Range (min … max):    19.3 ms …  24.4 ms    129 runs
 
Summary
  ./target/release/ruff check ./crates/ruff_linter/resources/test/cpython/ -e -s ran
    1.01 ± 0.06 times faster than ./target/release/ruff-fn check ./crates/ruff_linter/resources/test/cpython/ -e -s
    1.24 ± 0.08 times faster than ./target/release/ruff-main check ./crates/ruff_linter/resources/test/cpython/ -e -s

```


---

_Review requested from @carljm by @MichaReiser on 2024-07-09 12:55_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-09 12:55_

---

_Label `performance` added by @MichaReiser on 2024-07-09 12:56_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/resolver.rs`:387 on 2024-07-09 12:57_

New default for the number of threads

---

_@MichaReiser reviewed on 2024-07-09 12:57_

---

_Comment by @github-actions[bot] on 2024-07-09 13:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-07-09 15:50_

Sweet.

---

_Merged by @MichaReiser on 2024-07-10 07:29_

---

_Closed by @MichaReiser on 2024-07-10 07:29_

---

_Branch deleted on 2024-07-10 07:29_

---
