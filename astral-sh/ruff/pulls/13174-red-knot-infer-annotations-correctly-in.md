```yaml
number: 13174
title: "[red-knot] infer annotations correctly in assignments"
type: pull_request
state: closed
author: chriskrycho
labels: []
assignees: []
draft: true
base: main
head: rk-assignment-annotations
created_at: 2024-08-30T23:16:20Z
updated_at: 2024-09-04T21:57:15Z
url: https://github.com/astral-sh/ruff/pull/13174
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] infer annotations correctly in assignments

---

_Pull request opened by @chriskrycho on 2024-08-30 23:16_

## Summary

Switch over from `infer_expression` to `infer_annotation_expression` for annotations present in assignments, using the infrastructure built up in #13170.

## Test Plan

Updated the test to account for the better inference.

## Status

Depends on #13170. Without it, the result of inference here ends up being just `Unknown`. With it, it ends up being `Unknown | ellipsis`. Neither of these is *quite* right, but the latter is *closer* to being correct.

---

_Comment by @codspeed-hq[bot] on 2024-08-30 23:21_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/chriskrycho:rk-assignment-annotations)

### Merging #13174 will **degrade performances by 5.5%**

<sub>Comparing <code>chriskrycho:rk-assignment-annotations</code> (808bcd1) with <code>main</code> (28ab5f4)</sub>



### Summary

`❌ 1` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/chriskrycho:rk-assignment-annotations)._

### Benchmarks breakdown

|     | Benchmark | `main` | `chriskrycho:rk-assignment-annotations` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[numpy/globals.py]` | 726.4 µs | 768.7 µs | -5.5% |


---

_Closed by @chriskrycho on 2024-09-04 21:57_

---
