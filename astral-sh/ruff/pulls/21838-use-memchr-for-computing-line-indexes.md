```yaml
number: 21838
title: "Use `memchr` for computing line indexes"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/line-index
created_at: 2025-12-08T02:16:20Z
updated_at: 2025-12-08T13:50:54Z
url: https://github.com/astral-sh/ruff/pull/21838
synced_at: 2026-01-12T15:57:35Z
```

# Use `memchr` for computing line indexes

---

_@charliermarsh_

## Summary

Some benchmarks with Claude's help:

  | File                | Size  | Baseline             | Optimized            | Speedup |
  |---------------------|-------|----------------------|----------------------|---------|
  | numpy/globals.py    | 3 KB  | 1.48 µs (1.95 GiB/s) | 740 ns (3.89 GiB/s)  | 2.0x    |
  | unicode/pypinyin.py | 4 KB  | 2.04 µs (2.01 GiB/s) | 1.18 µs (3.49 GiB/s) | 1.7x    |
  | pydantic/types.py   | 26 KB | 13.1 µs (1.90 GiB/s) | 5.88 µs (4.23 GiB/s) | 2.2x    |
  | numpy/ctypeslib.py  | 17 KB | 8.45 µs (1.92 GiB/s) | 3.94 µs (4.13 GiB/s) | 2.1x    |
  | large/dataset.py    | 41 KB | 21.6 µs (1.84 GiB/s) | 11.2 µs (3.55 GiB/s) | 1.9x    |

I think that I originally thought we _had_ to iterate character-by-character here because we needed to do the ASCII check, but the ASCII check can be vectorized by LLVM (and the "search for newlines" can be done with `memchr`).


---

_Label `performance` added by @charliermarsh on 2025-12-08 02:16_

---

_Marked ready for review by @charliermarsh on 2025-12-08 02:16_

---

_Comment by @astral-sh-bot[bot] on 2025-12-08 02:35_


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

_Comment by @alex on 2025-12-08 04:34_

I doubt it's worth the effort (maybe some day, when std::simd is stable...) but I imagine in principle you could merge the two operations pretty easily:

- load chunk
- is_ascii |= chunk & splat(0x80)
- chunk == splat('\n') | chunk == splat('\r')
  - find the idx if it is

the win would memory locality, saving iterating 2x

---

_Merged by @charliermarsh on 2025-12-08 13:50_

---

_Closed by @charliermarsh on 2025-12-08 13:50_

---

_Branch deleted on 2025-12-08 13:50_

---
