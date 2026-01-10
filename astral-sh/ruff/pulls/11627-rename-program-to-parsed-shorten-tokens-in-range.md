```yaml
number: 11627
title: "Rename `Program` to `Parsed`, shorten `tokens_in_range`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/parsed-rename
created_at: 2024-05-31T04:49:48Z
updated_at: 2024-05-31T04:54:16Z
url: https://github.com/astral-sh/ruff/pull/11627
synced_at: 2026-01-10T21:56:00Z
```

# Rename `Program` to `Parsed`, shorten `tokens_in_range`

---

_Pull request opened by @dhruvmanila on 2024-05-31 04:49_

## Summary

This PR renames `Program` to `Parsed` and shortens the `tokens_in_range` to `in_range`.

The `Program` terminology is used in `red_knot` and this rename is to avoid the conflict. The `tokens_in_range` is usually used as `tokens.tokens_in_range` which suggests that the "tokens" word is repeated, so let's shorten it.


---

_Label `internal` added by @dhruvmanila on 2024-05-31 04:49_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-05-31 04:49_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-31 04:49_

---

_Merged by @dhruvmanila on 2024-05-31 04:49_

---

_Closed by @dhruvmanila on 2024-05-31 04:49_

---

_Branch deleted on 2024-05-31 04:49_

---

_Review request for @MichaReiser removed by @dhruvmanila on 2024-05-31 04:50_

---

_Review request for @AlexWaygood removed by @dhruvmanila on 2024-05-31 04:50_

---

_Comment by @codspeed-hq[bot] on 2024-05-31 04:54_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/parsed-rename)

### Merging #11627 will **improve performances by 28.24%**

<sub>Comparing <code>dhruv/parsed-rename</code> (d6a2035) with <code>main</code> (685d11a)</sub>



### Summary

`⚡ 19` improvements
`✅ 11` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `dhruv/parsed-rename` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `lexer[large/dataset.py]` | 1.4 ms | 1.1 ms | +27.64% |
| ⚡ | `lexer[numpy/ctypeslib.py]` | 284.3 µs | 225.8 µs | +25.93% |
| ⚡ | `lexer[numpy/globals.py]` | 36.7 µs | 29.6 µs | +24.09% |
| ⚡ | `lexer[pydantic/types.py]` | 639.9 µs | 502.9 µs | +27.23% |
| ⚡ | `lexer[unicode/pypinyin.py]` | 98.3 µs | 76.6 µs | +28.24% |
| ⚡ | `linter/all-rules[large/dataset.py]` | 16.9 ms | 15.9 ms | +6.5% |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 4.1 ms | 3.9 ms | +5.07% |
| ⚡ | `linter/all-rules[pydantic/types.py]` | 8.2 ms | 7.7 ms | +6.91% |
| ⚡ | `linter/default-rules[large/dataset.py]` | 4.3 ms | 3.7 ms | +16.42% |
| ⚡ | `linter/default-rules[numpy/ctypeslib.py]` | 1,013.7 µs | 941.2 µs | +7.71% |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 2.1 ms | 1.9 ms | +11.87% |
| ⚡ | `linter/default-rules[unicode/pypinyin.py]` | 406 µs | 359.7 µs | +12.86% |
| ⚡ | `linter/all-with-preview-rules[large/dataset.py]` | 20.3 ms | 19.2 ms | +5.69% |
| ⚡ | `linter/all-with-preview-rules[unicode/pypinyin.py]` | 2.4 ms | 2.3 ms | +4.37% |
| ⚡ | `parser[large/dataset.py]` | 5.7 ms | 5.2 ms | +10.14% |
| ⚡ | `parser[numpy/ctypeslib.py]` | 1,161.7 µs | 991.4 µs | +17.17% |
| ⚡ | `parser[numpy/globals.py]` | 122.2 µs | 105.8 µs | +15.47% |
| ⚡ | `parser[pydantic/types.py]` | 2.3 ms | 2.2 ms | +6.18% |
| ⚡ | `parser[unicode/pypinyin.py]` | 371.3 µs | 335.4 µs | +10.68% |


---
