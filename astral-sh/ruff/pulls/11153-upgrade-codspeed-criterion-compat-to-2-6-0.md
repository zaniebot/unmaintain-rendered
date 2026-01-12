```yaml
number: 11153
title: Upgrade codspeed-criterion-compat to 2.6.0
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ci
assignees: []
merged: true
base: main
head: codspeed-2.6
created_at: 2024-04-25T20:54:28Z
updated_at: 2024-04-26T00:22:37Z
url: https://github.com/astral-sh/ruff/pull/11153
synced_at: 2026-01-12T15:55:35Z
```

# Upgrade codspeed-criterion-compat to 2.6.0

---

_@ibraheemdev_

## Summary

`codspeed-criterion-compat` 2.6.0 [updates it's package selection mechanism](https://github.com/CodSpeedHQ/codspeed-rust/pull/43), so https://github.com/astral-sh/ruff/pull/10735 is no longer needed.

## Test Plan

CI should still be passing.

---

_Comment by @codspeed-hq[bot] on 2024-04-25 21:02_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheemdev:codspeed-2.6)

### Merging #11153 will **improve performances by 6.69%**

<sub>Comparing <code>ibraheemdev:codspeed-2.6</code> (61b9def) with <code>main</code> (dc09f52)</sub>



### Summary

`⚡ 3` improvements
`✅ 27` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `ibraheemdev:codspeed-2.6` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-with-preview-rules[unicode/pypinyin.py]` | 10.1 ms | 9.7 ms | +4.1% |
| ⚡ | `parser[numpy/ctypeslib.py]` | 5.6 ms | 5.3 ms | +6.69% |
| ⚡ | `parser[pydantic/types.py]` | 11.9 ms | 11.4 ms | +4.77% |


---

_Comment by @github-actions[bot] on 2024-04-25 21:14_

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

_@charliermarsh approved on 2024-04-25 21:42_

Thanks!

---

_Label `ci` added by @zanieb on 2024-04-25 22:43_

---

_Merged by @charliermarsh on 2024-04-26 00:22_

---

_Closed by @charliermarsh on 2024-04-26 00:22_

---
