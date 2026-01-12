```yaml
number: 12238
title: Warn about D203 formatter incompatibility
type: pull_request
state: merged
author: MichaReiser
labels:
  - cli
assignees: []
merged: true
base: main
head: warn-about-d203-formatter-incompatibility
created_at: 2024-07-08T08:17:03Z
updated_at: 2024-07-08T13:12:15Z
url: https://github.com/astral-sh/ruff/pull/12238
synced_at: 2026-01-12T15:55:40Z
```

# Warn about D203 formatter incompatibility

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/12220

## Test Plan

**Select all**

```
❯ cargo run --bin ruff -- format ../test/test2.py  --no-cache
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.12s
     Running `target/debug/ruff format ../test/test2.py --no-cache`
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
1 file left unchanged
```

**With `D211` ignored (and D203 enabled)**

```
ruff on  main [$!] is 📦 v0.5.1 via 🐍 v3.12.4 via 🦀 v1.79.0 
❯ cargo run --bin ruff -- format ../test/test2.py  --no-cache
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.12s
     Running `target/debug/ruff format ../test/test2.py --no-cache`
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
warning: The following rules may cause conflicts when used with the formatter: `D203`. To avoid unexpected behavior, we recommend disabling these rules, either by removing them from the `select` or `extend-select` configuration, or adding them to the `ignore` configuration.
1 file left unchanged

```


---

_Label `cli` added by @MichaReiser on 2024-07-08 08:17_

---

_Comment by @github-actions[bot] on 2024-07-08 08:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila approved on 2024-07-08 09:14_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-07-08 09:19_

---

_@charliermarsh approved on 2024-07-08 13:10_

---

_Merged by @MichaReiser on 2024-07-08 13:12_

---

_Closed by @MichaReiser on 2024-07-08 13:12_

---

_Branch deleted on 2024-07-08 13:12_

---
