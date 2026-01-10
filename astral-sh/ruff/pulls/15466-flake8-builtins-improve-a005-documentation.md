```yaml
number: 15466
title: "[`flake8-builtins`] Improve A005 documentation"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: a005-docs
created_at: 2025-01-14T00:54:31Z
updated_at: 2025-01-14T09:14:16Z
url: https://github.com/astral-sh/ruff/pull/15466
synced_at: 2026-01-10T20:34:00Z
```

# [`flake8-builtins`] Improve A005 documentation

---

_Pull request opened by @tjkuson on 2025-01-14 00:54_

## Summary

Adds an example to the diagnostic documentation.

Closes #15455

## Test Plan

Built the docs locally and checked the rendering.


---

_Comment by @github-actions[bot] on 2025-01-14 01:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @tjkuson on 2025-01-14 01:09_

Motivated by #15467, noticed that the example given

```
[tool.ruff.lint.flake8-builtins]
builtins-allowed-modules = ["id"]
```

isn't actually a module

```console
$ python3 -c 'import id'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
    import id
ModuleNotFoundError: No module named 'id'
```

so changed that too

---

_Label `documentation` added by @MichaReiser on 2025-01-14 07:40_

---

_@MichaReiser approved on 2025-01-14 07:42_

---

_Merged by @MichaReiser on 2025-01-14 07:42_

---

_Closed by @MichaReiser on 2025-01-14 07:42_

---

_Branch deleted on 2025-01-14 08:57_

---

_Comment by @AlexWaygood on 2025-01-14 09:14_

This is great, thank you!

---
