```yaml
number: 8657
title: "isort: Support disabling sections with ``no-sections = true``"
type: pull_request
state: merged
author: jelmer
labels:
  - isort
assignees: []
merged: true
base: main
head: isort-no-sections
created_at: 2023-11-13T17:39:17Z
updated_at: 2023-11-14T21:51:43Z
url: https://github.com/astral-sh/ruff/pull/8657
synced_at: 2026-01-12T15:55:26Z
```

# isort: Support disabling sections with ``no-sections = true``

---

_@jelmer_

## Summary

This adds a ``no-sections`` option for isort in the linter, similar to the ``no_sections`` option that exists in upstream isort (https://pycqa.github.io/isort/docs/configuration/options.html#no-sections)

This option puts all imports except for ``__future__`` into the same section, and is mostly used by monorepos.

I've taken a bit of a leap in assuming that ruff wants to support the exact same option; more than happy to refactor if you'd prefer a different way of setting this up.

Fixes #8653

## Test Plan

I've added a test and have run it on a large Python codebase that uses isort with --no-sections. The option is disabled by default.


---

_Comment by @github-actions[bot] on 2023-11-13 17:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @jelmer on 2023-11-13 18:06_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/isort/categorize.rs`:81 on 2023-11-13 20:41_

Does isort put the `__future__` imports in their own section, even if you pass `--no-sections`?

---

_@charliermarsh reviewed on 2023-11-13 20:41_

---

_@charliermarsh reviewed on 2023-11-13 20:41_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/options.rs`:2121 on 2023-11-13 20:41_

I think it'd be great to have some of these warnings here prior to merging.

---

_@charliermarsh reviewed on 2023-11-13 20:42_

Thanks for getting involved! No objection to adding this, and no objection to using the same nomenclature and API as isort. Left a few small questions and comments, but should be able to get this merged relatively quickly.

---

_@jelmer reviewed on 2023-11-13 20:45_

---

_Review comment by @jelmer on `crates/ruff_linter/src/rules/isort/categorize.rs`:81 on 2023-11-13 20:45_

Yes, I can reproduce that with isort itself - it explicitly special-cases ``__future__``, as can be seen here: https://github.com/PyCQA/isort/blob/c574c4c190ceddb66b10330ae811a95306f07b74/isort/output.py#L36

---

_Label `isort` added by @charliermarsh on 2023-11-14 00:17_

---

_@jelmer reviewed on 2023-11-14 11:37_

---

_Review comment by @jelmer on `crates/ruff_workspace/src/options.rs`:2121 on 2023-11-14 11:37_

I've added a warning for these two situations.

---

_Review requested from @charliermarsh by @jelmer on 2023-11-14 11:38_

---

_Renamed from "isort: Support disabling sections with ``no_sections = false``" to "isort: Support disabling sections with ``no-sections = true``" by @jelmer on 2023-11-14 11:38_

---

_@charliermarsh approved on 2023-11-14 18:10_

Thanks!

---

_Merged by @charliermarsh on 2023-11-14 21:45_

---

_Closed by @charliermarsh on 2023-11-14 21:45_

---
