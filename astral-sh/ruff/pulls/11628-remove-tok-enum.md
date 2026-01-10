```yaml
number: 11628
title: "Remove `Tok` enum"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/remove-tok
created_at: 2024-05-31T04:52:03Z
updated_at: 2024-05-31T04:57:00Z
url: https://github.com/astral-sh/ruff/pull/11628
synced_at: 2026-01-10T21:56:00Z
```

# Remove `Tok` enum

---

_Pull request opened by @dhruvmanila on 2024-05-31 04:52_

## Summary

This PR removes the `Tok` enum now that all of it's dependencies have been updated to use `TokenKind` instead.

closes: #11401


---

_Label `internal` added by @dhruvmanila on 2024-05-31 04:52_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-31 04:52_

---

_Merged by @dhruvmanila on 2024-05-31 04:52_

---

_Closed by @dhruvmanila on 2024-05-31 04:52_

---

_Branch deleted on 2024-05-31 04:52_

---

_Comment by @codspeed-hq[bot] on 2024-05-31 04:56_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/remove-tok)

### Merging #11628 will **improve performances by 28.24%**

<sub>Comparing <code>dhruv/remove-tok</code> (ed22238) with <code>main</code> (685d11a)</sub>



### Summary

`⚡ 19` improvements
`✅ 11` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `dhruv/remove-tok` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `lexer[large/dataset.py]` | 1.4 ms | 1.1 ms | +27.64% |
| ⚡ | `lexer[numpy/ctypeslib.py]` | 284.3 µs | 225.8 µs | +25.93% |
| ⚡ | `lexer[numpy/globals.py]` | 36.7 µs | 29.6 µs | +24.09% |
| ⚡ | `lexer[pydantic/types.py]` | 639.9 µs | 502.9 µs | +27.23% |
| ⚡ | `lexer[unicode/pypinyin.py]` | 98.3 µs | 76.6 µs | +28.24% |
| ⚡ | `linter/all-rules[large/dataset.py]` | 16.9 ms | 15.9 ms | +6.13% |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 4.1 ms | 3.9 ms | +5.44% |
| ⚡ | `linter/all-rules[pydantic/types.py]` | 8.2 ms | 7.7 ms | +6.91% |
| ⚡ | `linter/default-rules[large/dataset.py]` | 4.3 ms | 3.7 ms | +16.42% |
| ⚡ | `linter/default-rules[numpy/ctypeslib.py]` | 1,013.7 µs | 941.1 µs | +7.71% |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 2.1 ms | 1.9 ms | +11.87% |
| ⚡ | `linter/default-rules[unicode/pypinyin.py]` | 406 µs | 359.7 µs | +12.88% |
| ⚡ | `linter/all-with-preview-rules[large/dataset.py]` | 20.3 ms | 19.1 ms | +6.08% |
| ⚡ | `linter/all-with-preview-rules[pydantic/types.py]` | 10 ms | 9.6 ms | +4.25% |
| ⚡ | `parser[large/dataset.py]` | 5.7 ms | 5.2 ms | +10.18% |
| ⚡ | `parser[numpy/ctypeslib.py]` | 1,161.7 µs | 991.4 µs | +17.17% |
| ⚡ | `parser[numpy/globals.py]` | 122.2 µs | 105.8 µs | +15.47% |
| ⚡ | `parser[pydantic/types.py]` | 2.3 ms | 2.2 ms | +5.97% |
| ⚡ | `parser[unicode/pypinyin.py]` | 371.3 µs | 337.7 µs | +9.93% |


---
