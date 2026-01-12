```yaml
number: 9538
title: Ignore preview status for fixable and unfixable selectors
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - configuration
assignees: []
merged: true
base: main
head: charlie/fix
created_at: 2024-01-15T20:15:32Z
updated_at: 2024-01-16T02:51:40Z
url: https://github.com/astral-sh/ruff/pull/9538
synced_at: 2026-01-12T15:55:29Z
```

# Ignore preview status for fixable and unfixable selectors

---

_@charliermarsh_

## Summary

Right now, if you run with `explicit-preview-rules`, and use something like `select = ["RUF017"]`, we won't actually enable fixing for that rule, because `fixable = ["ALL"]` (the default) won't include `RUF017` due to the `explicit-preview-rules`.

The framing in this PR is that `explicit-preview-rules` should only affect the enablement selectors, whereas the fixable selectors should just include all possible matching rules. I think this will lead to the most intuitive behavior.

Closes https://github.com/astral-sh/ruff/issues/9282. (An alternative to https://github.com/astral-sh/ruff/pull/9284.)


---

_Review requested from @zanieb by @charliermarsh on 2024-01-15 20:15_

---

_@charliermarsh reviewed on 2024-01-15 20:16_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/configuration.rs`:789 on 2024-01-15 20:16_

If we only apply the change to the initial `let mut fixable_set: RuleSet = RuleSelector::All.all_rules().collect()`, and not here, confusing things can happen... E.g. if you used `select = ["RUF017"]`, then `unfixable = ["RUF"]` wouldn't mark `RUF017` as unfixable, which seems confusing.

---

_Label `bug` added by @charliermarsh on 2024-01-15 20:16_

---

_Label `configuration` added by @charliermarsh on 2024-01-15 20:16_

---

_Comment by @codspeed-hq[bot] on 2024-01-15 20:22_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/fix)

### Merging #9538 will **degrade performances by 4.33%**

<sub>Comparing <code>charlie/fix</code> (37f42fa) with <code>main</code> (f9331c7)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/fix)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/fix` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[numpy/ctypeslib.py]` | 12.2 ms | 12.8 ms | -4.33% |


---

_Comment by @github-actions[bot] on 2024-01-15 20:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@zanieb approved on 2024-01-15 21:00_

Yep thanks!

---

_Comment by @charliermarsh on 2024-01-15 21:03_

Gonna add a test prior to merging.

---

_Merged by @charliermarsh on 2024-01-16 02:48_

---

_Closed by @charliermarsh on 2024-01-16 02:48_

---

_Branch deleted on 2024-01-16 02:48_

---
