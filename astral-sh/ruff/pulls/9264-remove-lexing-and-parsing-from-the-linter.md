```yaml
number: 9264
title: Remove lexing and parsing from the linter benchmark
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/benchmark
created_at: 2023-12-23T21:29:42Z
updated_at: 2023-12-23T21:54:05Z
url: https://github.com/astral-sh/ruff/pull/9264
synced_at: 2026-01-12T15:55:28Z
```

# Remove lexing and parsing from the linter benchmark

---

_@charliermarsh_

## Summary

This PR adds some helper structs to the linter paths to enable passing in the pre-computed tokens and parsed source code during benchmarking, to remove lexing and parsing from the overall linter benchmark measurement. We already remove parsing for the formatter, and we have separate benchmarks for the lexer and the parser, so this should make it much easier to measure linter performance changes.

---

_Label `internal` added by @charliermarsh on 2023-12-23 21:30_

---

_Comment by @codspeed-hq[bot] on 2023-12-23 21:36_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/benchmark)

### Merging #9264 will **improve performances by ×4.9**

<sub>Comparing <code>charlie/benchmark</code> (30c83ae) with <code>main</code> (20def33)</sub>



### Summary

`⚡ 15` improvements
`✅ 15` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/benchmark` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[unicode/pypinyin.py]` | 6 ms | 1.5 ms | ×4.1 |
| ⚡ | `linter/default-rules[large/dataset.py]` | 92.1 ms | 19 ms | ×4.9 |
| ⚡ | `linter/default-rules[numpy/globals.py]` | 1,799.9 µs | 634.8 µs | ×2.8 |
| ⚡ | `linter/all-rules[large/dataset.py]` | 178 ms | 101.9 ms | +74.62% |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 36.4 ms | 8.4 ms | ×4.3 |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 4.5 ms | 3.4 ms | +31.16% |
| ⚡ | `linter/all-rules[pydantic/types.py]` | 77.1 ms | 48.2 ms | +60.21% |
| ⚡ | `linter/all-with-preview-rules[pydantic/types.py]` | 85.9 ms | 56.2 ms | +52.92% |
| ⚡ | `linter/all-with-preview-rules[unicode/pypinyin.py]` | 17.8 ms | 13.3 ms | +33.86% |
| ⚡ | `linter/all-with-preview-rules[numpy/ctypeslib.py]` | 39.4 ms | 26.3 ms | +49.92% |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 36.1 ms | 23.2 ms | +55.54% |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 4.2 ms | 3 ms | +39.02% |
| ⚡ | `linter/all-with-preview-rules[large/dataset.py]` | 197 ms | 122 ms | +61.46% |
| ⚡ | `linter/all-rules[unicode/pypinyin.py]` | 16.7 ms | 12.2 ms | +37.28% |
| ⚡ | `linter/default-rules[numpy/ctypeslib.py]` | 17.1 ms | 4 ms | ×4.3 |


---

_Merged by @charliermarsh on 2023-12-23 21:43_

---

_Closed by @charliermarsh on 2023-12-23 21:43_

---

_Branch deleted on 2023-12-23 21:43_

---

_Comment by @charliermarsh on 2023-12-23 21:43_

Just gonna merge since folks are out, I'll handle any feedback in follow-up PRs if it comes in.

---

_Comment by @github-actions[bot] on 2023-12-23 21:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: 'quote-style = preserve' is a preview only feature. Run with '--preview' to enable it.
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 1 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/mypy/blob/7d842e865331b3b6f44a116a886ea3c24eb67461/mypy/stubtest.py#L1396'>mypy/stubtest.py:1396:27:</a> E999 SyntaxError: unexpected EOF while parsing
- <a href='https://github.com/python/mypy/blob/7d842e865331b3b6f44a116a886ea3c24eb67461/mypy/stubtest.py#L680'>mypy/stubtest.py:680:21:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E999 | 1 | 1 | 0 | 0 | 0 |
| E721 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: 'quote-style = preserve' is a preview only feature. Run with '--preview' to enable it.
```

</p>
</details>

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @charliermarsh on 2023-12-23 21:54_

Not sure I understand the Mypy ecosystem change, hmm... (The `setuptools` error is already occurring on `main` and is unrelated.)

---
