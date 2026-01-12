```yaml
number: 10094
title: "E203: Don't warn about single whitespace before tuple ,"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
assignees: []
merged: true
base: main
head: e203-tuple-whitespace
created_at: 2024-02-23T10:05:47Z
updated_at: 2024-02-26T17:22:36Z
url: https://github.com/astral-sh/ruff/pull/10094
synced_at: 2026-01-12T15:55:31Z
```

# E203: Don't warn about single whitespace before tuple ,

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/10041

This PR fixes an issue in the `E203` rule where it disallowed whitespace before a `:` if the `:` was directly followed by a `,`:

```python
ham[lower + 1 :, "columnname"]
```

This conflicts with our formatter because it inserts a whitespace before the `:` for all *non simple* expressions. 

```python
dataframe.loc[index + 1 :, "columnname"]
dataframe.loc[index + 1 :]
```

This PR allows the whitespace before the colon if what follows after is a `,`, the same as we already do today if the colon is directly followed by a `]`. 


## Test Plan

Added test


---

_Review requested from @charliermarsh by @MichaReiser on 2024-02-23 10:05_

---

_Label `bug` added by @MichaReiser on 2024-02-23 10:06_

---

_Comment by @github-actions[bot] on 2024-02-23 10:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-02-26 15:47_

---

_Merged by @MichaReiser on 2024-02-26 17:22_

---

_Closed by @MichaReiser on 2024-02-26 17:22_

---

_Branch deleted on 2024-02-26 17:22_

---
