```yaml
number: 9480
title: Include base pyproject when initializing cache settings
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/resolver
created_at: 2024-01-12T00:11:43Z
updated_at: 2024-01-12T00:25:14Z
url: https://github.com/astral-sh/ruff/pull/9480
synced_at: 2026-01-12T15:55:29Z
```

# Include base pyproject when initializing cache settings

---

_@charliermarsh_

## Summary

Regression from https://github.com/astral-sh/ruff/pull/9453/files#diff-80a9c2637c432502a7075c792cc60db92282dd786999a78bfa9bb6f025afab35L482.

Closes https://github.com/astral-sh/ruff/issues/9478.

## Test Plan

```
rm -rf .ruff_cache
cargo run -p ruff_cli -- check ../foo.py
```

Failed prior to this PR; passes afterwards. The file must be outside of the current working directory, and must not have a `pyproject.toml` in any parent directory.


---

_Marked ready for review by @charliermarsh on 2024-01-12 00:11_

---

_Label `bug` added by @charliermarsh on 2024-01-12 00:11_

---

_Comment by @codspeed-hq[bot] on 2024-01-12 00:18_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/resolver)

### Merging #9480 will **improve performances by 4.55%**

<sub>Comparing <code>charlie/resolver</code> (1fd067d) with <code>main</code> (55f8f3b)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/resolver` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[numpy/ctypeslib.py]` | 12.8 ms | 12.2 ms | +4.55% |


---

_Merged by @charliermarsh on 2024-01-12 00:18_

---

_Closed by @charliermarsh on 2024-01-12 00:18_

---

_Branch deleted on 2024-01-12 00:18_

---

_Comment by @github-actions[bot] on 2024-01-12 00:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
