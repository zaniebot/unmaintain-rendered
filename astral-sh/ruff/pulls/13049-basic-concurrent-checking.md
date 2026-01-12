```yaml
number: 13049
title: Basic concurrent checking
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: concurrent=checking
created_at: 2024-08-22T09:00:42Z
updated_at: 2024-08-24T08:53:30Z
url: https://github.com/astral-sh/ruff/pull/13049
synced_at: 2026-01-12T15:55:42Z
```

# Basic concurrent checking

---

_@MichaReiser_

## Summary

This PR makes `workspace.check` to check the files concurrently. 

This PR doesn't explore more advanced scheduling. For example, we could try to not only schedule the workspace files 
but also the dependencies of those files. This would give us better performance where a project has few first-party modules but depends on many third-party modules. 

The main downside of concurrent checking is that the logs become harder to read, especially when using `-vvv` because the output of different threads is intertwined. I don't have a good solution for this yet other than using `RAYON_NUM_THREADS=1` to explicitly fall-back to a single thread. 

## Test Plan

Checking black goes down from 108ms to 34ms (12 cores)

### Main

```
black (cold)
Benchmark 1: knot
  Time (mean ± σ):     125.2 ms ±   3.8 ms    [User: 89.3 ms, System: 35.7 ms]
  Range (min … max):   117.7 ms … 133.3 ms    24 runs
 
  Warning: Ignoring non-zero exit code.
 
jinja (cold)
Benchmark 1: knot
  Time (mean ± σ):     139.1 ms ±   9.2 ms    [User: 100.1 ms, System: 38.8 ms]
  Range (min … max):   125.0 ms … 155.7 ms    22 runs
 
  Warning: Ignoring non-zero exit code.
 
isort (cold)
Benchmark 1: knot
  Time (mean ± σ):      85.2 ms ±   2.3 ms    [User: 60.1 ms, System: 25.0 ms]
  Range (min … max):    80.6 ms …  89.1 ms    32 runs
 
  Warning: Ignoring non-zero exit code.
```

### Concurrent

```
uv run benchmark  --min-runs=3 --knot
black (cold)
Benchmark 1: knot
  Time (mean ± σ):      36.1 ms ±   1.1 ms    [User: 125.6 ms, System: 71.7 ms]
  Range (min … max):    33.3 ms …  39.4 ms    77 runs
 
  Warning: Ignoring non-zero exit code.
 
jinja (cold)
Benchmark 1: knot
  Time (mean ± σ):      32.6 ms ±   1.5 ms    [User: 140.8 ms, System: 88.7 ms]
  Range (min … max):    29.5 ms …  36.4 ms    88 runs
 
  Warning: Ignoring non-zero exit code.
 
isort (cold)
Benchmark 1: knot
  Time (mean ± σ):      30.7 ms ±   1.2 ms    [User: 82.4 ms, System: 46.7 ms]
  Range (min … max):    28.4 ms …  34.4 ms    89 runs
 
  Warning: Ignoring non-zero exit code.
 ```

---

_Label `red-knot` added by @MichaReiser on 2024-08-22 09:00_

---

_Comment by @codspeed-hq[bot] on 2024-08-22 09:06_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/concurrent=checking)

### Merging #13049 will **not alter performance**

<sub>Comparing <code>concurrent=checking</code> (acbed7c) with <code>main</code> (1f2cb09)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-08-22 09:20_

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

_Renamed from "A very basic prototype for concurrent checking" to "Basic concurrent checking" by @MichaReiser on 2024-08-23 07:21_

---

_Marked ready for review by @MichaReiser on 2024-08-23 08:17_

---

_Review requested from @carljm by @MichaReiser on 2024-08-23 08:17_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-23 08:17_

---

_@carljm approved on 2024-08-23 22:54_

Awesome!

---

_Merged by @MichaReiser on 2024-08-24 08:53_

---

_Closed by @MichaReiser on 2024-08-24 08:53_

---

_Branch deleted on 2024-08-24 08:53_

---
