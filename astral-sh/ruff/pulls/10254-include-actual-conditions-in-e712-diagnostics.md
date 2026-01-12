```yaml
number: 10254
title: Include actual conditions in E712 diagnostics
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
assignees: []
merged: true
base: main
head: improve-e712-message
created_at: 2024-03-06T19:23:12Z
updated_at: 2024-03-09T01:19:47Z
url: https://github.com/astral-sh/ruff/pull/10254
synced_at: 2026-01-12T15:55:31Z
```

# Include actual conditions in E712 diagnostics

---

_@tjkuson_

## Summary

Changes the generic recommendation to replace

```python
if foo == True: ...
```

with `if cond:` to `if foo:`.

Still uses a generic message for compound comparisons as a specific message starts to become confusing. For example,

```python
if foo == True != False: ...
```

produces two recommendations, one of which would recommend `if True:`, which is confusing.

Resolves [recommendation in a previous PR](https://github.com/astral-sh/ruff/pull/8613/files#r1514915070).

## Test Plan

`cargo nextest run`

---

_Comment by @github-actions[bot] on 2024-03-06 19:36_

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

_Marked ready for review by @tjkuson on 2024-03-06 19:46_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-03-06 21:45_

---

_Label `rule` added by @charliermarsh on 2024-03-06 21:45_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-06 21:45_

---

_@charliermarsh reviewed on 2024-03-06 21:50_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pycodestyle/rules/literal_comparisons.rs`:108 on 2024-03-06 21:50_

I think we should use `SourceCodeSnippet` for this -- can you give that a try? It will ensure that we don't use the name if it's too long or spans multiple lines, since that tends to yield poor diagnostic messages.

---

_@tjkuson reviewed on 2024-03-07 19:28_

---

_Review comment by @tjkuson on `crates/ruff_linter/src/rules/pycodestyle/rules/literal_comparisons.rs`:108 on 2024-03-07 19:28_

Sure, updated the branch.

---

_@charliermarsh reviewed on 2024-03-07 22:10_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pycodestyle/rules/literal_comparisons.rs`:108 on 2024-03-07 22:10_

Thanks!

---

_@charliermarsh approved on 2024-03-08 01:13_

Thanks!

---

_Merged by @charliermarsh on 2024-03-08 01:20_

---

_Closed by @charliermarsh on 2024-03-08 01:20_

---

_Branch deleted on 2024-03-09 01:19_

---
