```yaml
number: 12635
title: "[red-knot] update benchmark to run on tomllib"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/redbench
created_at: 2024-08-02T17:26:21Z
updated_at: 2024-08-02T18:23:54Z
url: https://github.com/astral-sh/ruff/pull/12635
synced_at: 2026-01-12T15:55:41Z
```

# [red-knot] update benchmark to run on tomllib

---

_@carljm_

Changes the red-knot benchmark to run on the stdlib "tomllib" library (which is self-contained, four files, uses type annotations) instead of on very small bits of handwritten code.

Also remove the `without_parse` benchmark: now that we are running on real code that uses typeshed, we'd either have to pre-parse all of typeshed (slow) or find some way to determine which typeshed modules will be used by the benchmark (not feasible with reasonable complexity.)

## Test Plan

`cargo bench -p ruff_benchmark --bench red_knot`

---

_Label `red-knot` added by @carljm on 2024-08-02 17:26_

---

_Review requested from @MichaReiser by @carljm on 2024-08-02 17:26_

---

_Review requested from @AlexWaygood by @carljm on 2024-08-02 17:26_

---

_@MichaReiser reviewed on 2024-08-02 17:29_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/red_knot.rs`:122 on 2024-08-02 17:29_

Keep? Remove? Is there any other check we can do to assert that we didn't outright fail? E.g. we had it in the past that we didn't download the actual source but instead the HTML file. That obviously didn't measure what we intended.

---

_@carljm reviewed on 2024-08-02 17:30_

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:122 on 2024-08-02 17:30_

My plan was to re-add this once we actually lint tomllib cleanly. I think that should happen with our existing goals for this month; it's mostly "undefined name" warnings coming from places we don't create definitions yet where we should (function arguments, for example.)

---

_Comment by @codspeed-hq[bot] on 2024-08-02 17:32_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm/redbench)

### Merging #12635 will **degrade performances by 86.42%**

<sub>Comparing <code>cjm/redbench</code> (765df65) with <code>main</code> (12177a4)</sub>



### Summary

`❌ 2 (👁 2)` regressions
`✅ 30` untouched benchmarks

`⁉️ 1 (👁 1)` dropped benchmarks



### Benchmarks breakdown

|     | Benchmark | `main` | `cjm/redbench` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `red_knot_check_file[cold]` | 14.2 ms | 40.2 ms | -64.62% |
| 👁 | `red_knot_check_file[incremental]` | 264.7 µs | 1,948.9 µs | -86.42% |
| 👁 | `red_knot_check_file[without_parse]` | 6.8 ms | N/A | N/A |


---

_Comment by @carljm on 2024-08-02 17:32_

One thing to note here is that the "without parse" benchmark becomes less accurately named; we pre-parse builtins and the first party code, but there's no simple way to know in advance which parts of typeshed we end up pulling in, so we don't pre-parse any other typeshed modules used.

One way to fix this would be to pre-parse all of typeshed? But I'm also not sure, as our benchmark gets bigger, if it's really important to have a benchmark that excludes parsing.

---

_Comment by @MichaReiser on 2024-08-02 17:33_

Tss, regressing performance.

I'm okay with removing the `without_parse` benchmark.  The incremental hit is slightly concerning. It's still fast with 3ms vs 47 but not as fast.

---

_@MichaReiser reviewed on 2024-08-02 17:34_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/red_knot.rs`:122 on 2024-08-02 17:34_

Can we hard-code the expected length for now? Or do something else to assert at least that the output matches our expectations? I'm otherwise worried that we see a huge perf improvement and the only reason for it is because we broke the benchmark :rofl: (happened before)

---

_@MichaReiser approved on 2024-08-02 17:36_

Nice work on figuring out and fixing the salsa validation bug. I would love if we can find some way to assert that the benchmarks run correctly. 

---

_@carljm reviewed on 2024-08-02 17:38_

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:122 on 2024-08-02 17:38_

That motivation makes sense. The downside is that for the rest of this month, a lot of PRs improving type inference will seem to work locally, get pushed to CI, and then benchmarks will fail on CI because the number of errors reduced.

---

_@AlexWaygood reviewed on 2024-08-02 17:39_

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/red_knot.rs`:122 on 2024-08-02 17:39_

I'd prefer it if we could add a comment to say why there's commented-out code

---

_Comment by @carljm on 2024-08-02 17:40_

I removed `without_parse`, and re-added an assertion on error count. I'm fine with that for now, we can re-assess if it becomes too painful.

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/red_knot.rs`:38 on 2024-08-02 17:41_

Is there a reason why we're excluding `__init__.py`? It might be nice to directly mimic the normal structure of a Python workspace here, where you'd have the package name nested inside the `src` directory:

```suggestion
    let init_path = SystemPath::new("/src/tomllib/__init__.py");
    let parser_path = SystemPath::new("/src/tomllib/parser.py");
    let re_path = SystemPath::new("/src/tomllib/_re.py");
    let types_path = SystemPath::new("/src/tomllib/_types.py");
```

---

_@AlexWaygood approved on 2024-08-02 17:41_

Congrats on tracking down and fixing the Salsa bug!

---

_@MichaReiser reviewed on 2024-08-02 17:42_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/red_knot.rs`:122 on 2024-08-02 17:42_

> That motivation makes sense. The downside is that for the rest of this month, a lot of PRs improving type inference will seem to work locally, get pushed to CI, and then benchmarks will fail on CI because the number of errors reduced.

Yeah that's a bit annoying but it also demonstrates progress :) 

---

_@carljm reviewed on 2024-08-02 17:47_

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:38 on 2024-08-02 17:47_

Just simplicity; `__init__.py` contains very little code and I didn't think it would add value to the benchmark that makes it worth dealing with four files instead of three.

But: I just realized that my approach of treating them as three separate top-level modules is broken; even when we add support for relative imports (which is going to look like a regression, because suddenly the imports between these files will start working) those imports would be invalid relative imports because they aren't inside a package.

So yeah, good catch: I'll update to put these inside a package and include `__init__.py`.


---

_@carljm reviewed on 2024-08-02 17:51_

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:122 on 2024-08-02 17:51_

In the latest version there's no commented-out code anymore.

---

_Comment by @carljm on 2024-08-02 18:00_

I am really not sure why the changes I just pushed (removing `without_parse` and putting all the code inside a `tomllib` package with `__init__.py`) made the incremental benchmark go from 10x slower to 2.5x faster...

---

_Comment by @carljm on 2024-08-02 18:02_

Oh! I see why; writing to the wrong path for `_re.py` now :/ Fixing.

Good to know that re-checking with no changes is pretty fast :)

---

_Comment by @github-actions[bot] on 2024-08-02 18:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @carljm on 2024-08-02 18:23_

---

_Closed by @carljm on 2024-08-02 18:23_

---

_Branch deleted on 2024-08-02 18:23_

---
