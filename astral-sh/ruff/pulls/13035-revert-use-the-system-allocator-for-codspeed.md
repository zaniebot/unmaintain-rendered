```yaml
number: 13035
title: "Revert \"Use the system allocator for codspeed benchmarks\""
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: revert-13005-use-system-allocator-for-codspeed
created_at: 2024-08-21T14:46:36Z
updated_at: 2024-08-21T15:13:13Z
url: https://github.com/astral-sh/ruff/pull/13035
synced_at: 2026-01-10T21:38:32Z
```

# Revert "Use the system allocator for codspeed benchmarks"

---

_Pull request opened by @MichaReiser on 2024-08-21 14:46_

Reverts astral-sh/ruff#13005

I don't see any improvements. I think it's even worse then before.

---

_Label `ci` added by @MichaReiser on 2024-08-21 14:46_

---

_@AlexWaygood approved on 2024-08-21 14:47_

---

_Comment by @codspeed-hq[bot] on 2024-08-21 14:56_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/revert-13005-use-system-allocator-for-codspeed)

### Merging #13035 will **degrade performances by 9.74%**

<sub>Comparing <code>revert-13005-use-system-allocator-for-codspeed</code> (f7103b7) with <code>main</code> (ecd9e6a)</sub>



### Summary

`⚡ 9` improvements
`❌ 1 (👁 1)` regressions
`✅ 22` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `revert-13005-use-system-allocator-for-codspeed` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[large/dataset.py]` | 3.9 ms | 3.7 ms | +5.62% |
| 👁 | `linter/default-rules[numpy/globals.py]` | 170.5 µs | 189 µs | -9.74% |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 1.9 ms | 1.8 ms | +5.57% |
| ⚡ | `linter/all-with-preview-rules[pydantic/types.py]` | 10.2 ms | 9.7 ms | +5.16% |
| ⚡ | `linter/all-with-preview-rules[unicode/pypinyin.py]` | 2.5 ms | 2.4 ms | +4.48% |
| ⚡ | `parser[large/dataset.py]` | 5.6 ms | 4.9 ms | +12.86% |
| ⚡ | `parser[numpy/ctypeslib.py]` | 998.5 µs | 910.7 µs | +9.64% |
| ⚡ | `parser[numpy/globals.py]` | 112.7 µs | 101.6 µs | +10.95% |
| ⚡ | `parser[pydantic/types.py]` | 2.1 ms | 2 ms | +6.73% |
| ⚡ | `parser[unicode/pypinyin.py]` | 350.6 µs | 308.9 µs | +13.5% |


---

_Comment by @github-actions[bot] on 2024-08-21 15:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @MichaReiser on 2024-08-21 15:13_

---

_Closed by @MichaReiser on 2024-08-21 15:13_

---

_Branch deleted on 2024-08-21 15:13_

---
