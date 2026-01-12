```yaml
number: 9612
title: "[RUF] - Add unnecessary dict comprehension for iterable rule (RUF023)"
type: pull_request
state: closed
author: mikeleppane
labels: []
assignees: []
base: main
head: add-RUF023
created_at: 2024-01-22T11:37:02Z
updated_at: 2024-01-22T12:27:24Z
url: https://github.com/astral-sh/ruff/pull/9612
synced_at: 2026-01-12T15:55:29Z
```

# [RUF] - Add unnecessary dict comprehension for iterable rule (RUF023)

---

_@mikeleppane_

## Summary

Checks for unnecessary `dict` comprehension when creating a new dictionary from iterable. Suggest to replace with `dict.fromkeys(iterable)`

See: https://github.com/astral-sh/ruff/issues/9592

## Test Plan

```bash
cargo test
```


---

_Comment by @codspeed-hq[bot] on 2024-01-22 12:10_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mikeleppane:add-RUF023)

### Merging #9612 will **degrade performances by 4.61%**

<sub>Comparing <code>mikeleppane:add-RUF023</code> (9e7b476) with <code>main</code> (a3d667e)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/mikeleppane:add-RUF023)._

### Benchmarks breakdown

|     | Benchmark | `main` | `mikeleppane:add-RUF023` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-with-preview-rules[numpy/ctypeslib.py]` | 26.4 ms | 27.7 ms | -4.61% |


---

_Closed by @mikeleppane on 2024-01-22 12:27_

---
