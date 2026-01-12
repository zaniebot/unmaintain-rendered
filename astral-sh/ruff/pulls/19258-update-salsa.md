```yaml
number: 19258
title: Update salsa
type: pull_request
state: merged
author: ibraheemdev
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: ibraheem/update-salsa
created_at: 2025-07-10T12:51:23Z
updated_at: 2025-07-18T16:14:29Z
url: https://github.com/astral-sh/ruff/pull/19258
synced_at: 2026-01-12T15:56:35Z
```

# Update salsa

---

_@ibraheemdev_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Pulls in https://github.com/salsa-rs/salsa/pull/934.

---

_Label `internal` added by @ibraheemdev on 2025-07-10 12:51_

---

_Label `ty` added by @ibraheemdev on 2025-07-10 12:51_

---

_Comment by @github-actions[bot] on 2025-07-10 12:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-07-10 13:01_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fupdate-salsa?runnerMode=Instrumentation)

### Merging #19258 will **improve performances by 21.13%**

<sub>Comparing <code>ibraheem/update-salsa</code> (d4a32dd) with <code>main</code> (99d0ac6)</sub>



### Summary

`⚡ 9` improvements  
`✅ 32` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` ty_check_file[cold] `` | 125.8 ms | 115.9 ms | +8.53% |
| ⚡ | `` ty_check_file[incremental] `` | 5.6 ms | 4.9 ms | +14.57% |
| ⚡ | `` ty_micro[many_enum_members] `` | 112.2 ms | 105.7 ms | +6.18% |
| ⚡ | `` ty_micro[many_string_assignments] `` | 74.7 ms | 71.5 ms | +4.51% |
| ⚡ | `` ty_micro[many_tuple_assignments] `` | 141.5 ms | 116.9 ms | +21.13% |
| ⚡ | `` anyio `` | 915.4 ms | 827 ms | +10.69% |
| ⚡ | `` attrs `` | 398.6 ms | 362.7 ms | +9.88% |
| ⚡ | `` DateType `` | 249.2 ms | 230.8 ms | +8.01% |
| ⚡ | `` hydra-zen `` | 870.7 ms | 786.3 ms | +10.74% |


---

_Comment by @codspeed-hq[bot] on 2025-07-10 13:02_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fupdate-salsa?runnerMode=WallTime)

### Merging #19258 will **improve performances by 22.3%**

<sub>Comparing <code>ibraheem/update-salsa</code> (d4a32dd) with <code>main</code> (99d0ac6)</sub>



### Summary

`⚡ 7` improvements  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` large[sympy] `` | 54.7 s | 42.5 s | +28.71% |
| ⚡ | `` medium[colour-science] `` | 9.1 s | 7.1 s | +28.54% |
| ⚡ | `` medium[pandas] `` | 36.6 s | 28.8 s | +26.73% |
| ⚡ | `` small[altair] `` | 3 s | 2.4 s | +26.37% |
| ⚡ | `` small[freqtrade] `` | 4.9 s | 4 s | +22.3% |
| ⚡ | `` small[pydantic] `` | 2.9 s | 2.3 s | +27.96% |
| ⚡ | `` small[tanjun] `` | 2.1 s | 1.7 s | +23.76% |


---

_Comment by @github-actions[bot] on 2025-07-10 13:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_Marked ready for review by @ibraheemdev on 2025-07-18 14:52_

---

_Review requested from @carljm by @ibraheemdev on 2025-07-18 14:52_

---

_Review requested from @MichaReiser by @ibraheemdev on 2025-07-18 14:52_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-07-18 14:52_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-07-18 14:52_

---

_Review requested from @dcreager by @ibraheemdev on 2025-07-18 14:52_

---

_@MichaReiser approved on 2025-07-18 15:51_

Nice. 

Can you try running the playgroudn locally before merging. 

To do this:

* navigate to the `playground` directory
* Run `npm install`
* Run `npm start --workspace ty-playground`
* Open the link printed on the CLI

---

_Comment by @ibraheemdev on 2025-07-18 16:13_

Seems to work fine.

---

_Merged by @ibraheemdev on 2025-07-18 16:14_

---

_Closed by @ibraheemdev on 2025-07-18 16:14_

---

_Branch deleted on 2025-07-18 16:14_

---
