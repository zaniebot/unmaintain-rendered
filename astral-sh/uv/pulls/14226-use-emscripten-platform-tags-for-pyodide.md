```yaml
number: 14226
title: Use emscripten platform tags for Pyodide
type: pull_request
state: closed
author: charliermarsh
labels:
  - do-not-merge
assignees: []
draft: true
base: main
head: charlie/em
created_at: 2025-06-24T01:21:24Z
updated_at: 2025-06-30T23:43:35Z
url: https://github.com/astral-sh/uv/pull/14226
synced_at: 2026-01-10T06:53:01Z
```

# Use emscripten platform tags for Pyodide

---

_Pull request opened by @charliermarsh on 2025-06-24 01:21_

## Summary

I think the platform tag for Pyodide 2024 wheels is `emscripten_3_1_58_wasm32`. As far as I can tell, the `pyodide_2024_0_wasm32` piece doesn't actually show up in the wheels themselves, though I could be misunderstanding.

Builds on https://github.com/astral-sh/uv/pull/12731.

## Test Plan

```
cargo run pip install --python .venv-pyodide --target src/vendor https://github.com/pydantic/pydantic-core/releases/download/v2.35.1/pydantic_core-2.35.1-cp312-cp312-emscripten_3_1_58_wasm32.whl
```

---

_Comment by @charliermarsh on 2025-06-24 01:31_

Nevermind, it looks like some pyodide wheels _do_ use `pyodide_2024_0_wasm32`, so perhaps we should support both?

---

_Label `do-not-merge` added by @charliermarsh on 2025-06-24 01:34_

---

_Converted to draft by @charliermarsh on 2025-06-24 01:34_

---

_Comment by @codspeed-hq[bot] on 2025-06-24 01:36_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Fem?runnerMode=WallTime)

### Merging #14226 will **degrade performances by 17.14%**

<sub>Comparing <code>charlie/em</code> (0cdfb39) with <code>main</code> (aa2448e)</sub>



### Summary

`❌ 1` regressions  
`✅ 11` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/charlie%2Fem?runnerMode=WallTime)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` wheelname_parsing[flyte-long-compatible] `` | 1.3 µs | 1.6 µs | -17.14% |


---

_Closed by @charliermarsh on 2025-06-30 23:43_

---
