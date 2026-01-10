```yaml
number: 19020
title: Update Salsa
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/salsa-update
created_at: 2025-06-28T20:12:46Z
updated_at: 2025-07-02T13:55:39Z
url: https://github.com/astral-sh/ruff/pull/19020
synced_at: 2026-01-10T18:33:12Z
```

# Update Salsa

---

_Pull request opened by @MichaReiser on 2025-06-28 20:12_

## Summary

This PR updates Salsa to pull in Ibraheems multithreading improvements

https://github.com/salsa-rs/salsa/pull/921

## Performance

A small regression for single-threaded benchmarks is expected because papaya is slightly slower than a `Mutex<FxHashMap>` in the uncontested case (~10%). However, this shouldn't matter as much in practice because:

1. Salsa has a fast-path when only using 1 DB instance which is the common case in production. This fast-path is not impacted by the changes but we measure the slow paths in our benchmarks (because we use multiple db instances)
2. Fixing the 10x slowdown for the congested case (multi threading) outweights the downsides of a 10% perf regression for single threaded use cases, especially considering that ty is heavily multi threaded.

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2025-06-28 20:12_

---

_Label `ty` added by @MichaReiser on 2025-06-28 20:12_

---

_Review requested from @ibraheemdev by @MichaReiser on 2025-06-28 20:13_

---

_Comment by @MichaReiser on 2025-06-28 20:22_

@ibraheemdev can you take over this PR. It seems something broke the heap size feature upstream (maybe https://github.com/salsa-rs/salsa/pull/927?)

Edit: I pinned the commit to before said commit for now but that also means that the papaya commit is missing too. We should add a test in upstream salsa that catches the regression and fix it before merging this PR

---

_Assigned to @ibraheemdev by @MichaReiser on 2025-06-28 20:26_

---

_Comment by @github-actions[bot] on 2025-06-28 20:28_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
- TOTAL MEMORY USAGE: ~49MB
+ TOTAL MEMORY USAGE: ~45MB

beartype (https://github.com/beartype/beartype)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~97MB

discord.py (https://github.com/Rapptz/discord.py)
-     memo fields = ~189MB
+     memo fields = ~207MB

alerta (https://github.com/alerta/alerta)
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~106MB

isort (https://github.com/pycqa/isort)
- TOTAL MEMORY USAGE: ~80MB
+ TOTAL MEMORY USAGE: ~88MB

paasta (https://github.com/yelp/paasta)
-     memo fields = ~156MB
+     memo fields = ~171MB

django-stubs (https://github.com/typeddjango/django-stubs)
-     memo fields = ~156MB
+     memo fields = ~142MB

static-frame (https://github.com/static-frame/static-frame)
-     memo fields = ~304MB
+     memo fields = ~334MB

openlibrary (https://github.com/internetarchive/openlibrary)
-     memo fields = ~171MB
+     memo fields = ~189MB

rotki (https://github.com/rotki/rotki)
-     memo fields = ~445MB
+     memo fields = ~490MB

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- TOTAL MEMORY USAGE: ~717MB
+ TOTAL MEMORY USAGE: ~652MB

```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-06-28 20:34_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fsalsa-update?runnerMode=Instrumentation)

### Merging #19020 will **degrade performances by 7.34%**

<sub>Comparing <code>micha/salsa-update</code> (baee7c3) with <code>main</code> (ebc70a4)</sub>



### Summary

`❌ 1` regressions  
`✅ 38` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha%2Fsalsa-update?runnerMode=Instrumentation)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` ty_micro[many_tuple_assignments] `` | 128.9 ms | 139.2 ms | -7.34% |


---

_Comment by @codspeed-hq[bot] on 2025-06-28 20:36_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fsalsa-update?runnerMode=WallTime)

### Merging #19020 will **improve performances by ×8.5**

<sub>Comparing <code>micha/salsa-update</code> (baee7c3) with <code>main</code> (ebc70a4)</sub>



### Summary

`⚡ 1` improvements  
`✅ 7` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` multithreaded[pydantic] `` | 2,947.7 ms | 346.6 ms | ×8.5 |


---

_Comment by @github-actions[bot] on 2025-06-28 20:36_

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

_Comment by @MichaReiser on 2025-06-28 20:37_

Hmm, the memory usage changes here must be noise?

---

_Marked ready for review by @ibraheemdev on 2025-07-02 13:36_

---

_Merged by @ibraheemdev on 2025-07-02 13:55_

---

_Closed by @ibraheemdev on 2025-07-02 13:55_

---

_Branch deleted on 2025-07-02 13:55_

---
