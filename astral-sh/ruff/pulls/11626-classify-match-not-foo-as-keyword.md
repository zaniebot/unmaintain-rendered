```yaml
number: 11626
title: "Classify `match not foo` as keyword"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - testing
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/match-not
created_at: 2024-05-31T04:45:52Z
updated_at: 2024-05-31T04:50:31Z
url: https://github.com/astral-sh/ruff/pull/11626
synced_at: 2026-01-12T15:55:38Z
```

# Classify `match not foo` as keyword

---

_@dhruvmanila_

## Summary

This PR fixes a bug to classify `match` as a keyword for:
```py
match not foo:
    case _: ...
```

## Test Plan

This PR also adds a bunch of test cases around the classification of `match` token as a keyword or an identifier, including speculative parsing.


---

_Label `bug` added by @dhruvmanila on 2024-05-31 04:45_

---

_Label `testing` added by @dhruvmanila on 2024-05-31 04:45_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-31 04:45_

---

_@dhruvmanila reviewed on 2024-05-31 04:46_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:3576 on 2024-05-31 04:46_

The bug fix.

---

_Merged by @dhruvmanila on 2024-05-31 04:46_

---

_Closed by @dhruvmanila on 2024-05-31 04:46_

---

_Branch deleted on 2024-05-31 04:46_

---

_Comment by @codspeed-hq[bot] on 2024-05-31 04:50_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/match-not)

### Merging #11626 will **improve performances by 28.14%**

<sub>Comparing <code>dhruv/match-not</code> (2c10479) with <code>main</code> (685d11a)</sub>



### Summary

`⚡ 19` improvements
`✅ 11` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `dhruv/match-not` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `lexer[large/dataset.py]` | 1.4 ms | 1.1 ms | +27.63% |
| ⚡ | `lexer[numpy/ctypeslib.py]` | 284.3 µs | 225.8 µs | +25.9% |
| ⚡ | `lexer[numpy/globals.py]` | 36.7 µs | 29.7 µs | +23.73% |
| ⚡ | `lexer[pydantic/types.py]` | 639.9 µs | 503 µs | +27.22% |
| ⚡ | `lexer[unicode/pypinyin.py]` | 98.3 µs | 76.7 µs | +28.14% |
| ⚡ | `linter/all-rules[large/dataset.py]` | 16.9 ms | 15.9 ms | +6.15% |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 4.1 ms | 3.9 ms | +4.97% |
| ⚡ | `linter/all-rules[pydantic/types.py]` | 8.2 ms | 7.6 ms | +7.02% |
| ⚡ | `linter/all-rules[unicode/pypinyin.py]` | 2.1 ms | 2 ms | +4.64% |
| ⚡ | `linter/default-rules[large/dataset.py]` | 4.3 ms | 3.6 ms | +18.49% |
| ⚡ | `linter/default-rules[numpy/ctypeslib.py]` | 1,013.7 µs | 941.3 µs | +7.69% |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 2.1 ms | 1.9 ms | +11.85% |
| ⚡ | `linter/default-rules[unicode/pypinyin.py]` | 406 µs | 359.6 µs | +12.89% |
| ⚡ | `linter/all-with-preview-rules[large/dataset.py]` | 20.3 ms | 19.3 ms | +4.94% |
| ⚡ | `parser[large/dataset.py]` | 5.7 ms | 5.2 ms | +10.19% |
| ⚡ | `parser[numpy/ctypeslib.py]` | 1,161.7 µs | 991.5 µs | +17.17% |
| ⚡ | `parser[numpy/globals.py]` | 122.2 µs | 105.9 µs | +15.44% |
| ⚡ | `parser[pydantic/types.py]` | 2.3 ms | 2.2 ms | +5.97% |
| ⚡ | `parser[unicode/pypinyin.py]` | 371.3 µs | 337.8 µs | +9.92% |


---
