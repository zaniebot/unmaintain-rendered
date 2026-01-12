```yaml
number: 9558
title: "Make `ruff` the default binary"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/default
created_at: 2024-01-16T22:50:22Z
updated_at: 2024-01-17T14:14:44Z
url: https://github.com/astral-sh/ruff/pull/9558
synced_at: 2026-01-12T15:55:29Z
```

# Make `ruff` the default binary

---

_@charliermarsh_

## Summary

This makes `cargo run` equivalent to `cargo run -p ruff` which is almost always what you want.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-01-16 22:50_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-01-16 22:50_

---

_Label `internal` added by @charliermarsh on 2024-01-16 22:50_

---

_Comment by @codspeed-hq[bot] on 2024-01-16 22:57_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/default)

### Merging #9558 will **improve performances by 4.55%**

<sub>Comparing <code>charlie/default</code> (604e926) with <code>main</code> (8118d29)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/default` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[numpy/ctypeslib.py]` | 12.8 ms | 12.2 ms | +4.55% |


---

_Comment by @github-actions[bot] on 2024-01-16 23:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@zanieb approved on 2024-01-16 23:08_

---

_Merged by @charliermarsh on 2024-01-16 23:31_

---

_Closed by @charliermarsh on 2024-01-16 23:31_

---

_Branch deleted on 2024-01-16 23:31_

---

_Review comment by @BurntSushi on `crates/ruff/Cargo.toml`:13 on 2024-01-17 14:14_

TIL about `default-run`. Nice.

---

_@BurntSushi reviewed on 2024-01-17 14:14_

---
