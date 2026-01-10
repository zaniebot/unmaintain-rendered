```yaml
number: 10494
title: Spruce up docs for flake8-pyi rules (part 2)
type: pull_request
state: merged
author: AlexWaygood
labels:
  - documentation
assignees: []
merged: true
base: main
head: pyi-docs-2
created_at: 2024-03-20T17:37:50Z
updated_at: 2024-03-21T11:54:44Z
url: https://github.com/astral-sh/ruff/pull/10494
synced_at: 2026-01-10T22:47:02Z
```

# Spruce up docs for flake8-pyi rules (part 2)

---

_Pull request opened by @AlexWaygood on 2024-03-20 17:37_

A followup PR to #10422, auditing all the docs for our PYI rules that I didn't get to in that PR.

Summary of the changes:
- Improve clarity over the motivation for some rules
- Improve links to external references. In particular, reduce links to PEPs, as PEPs are generally historical documents rather than pieces of living documentation. Where possible, it's better to link to the [official typing spec](https://typing.readthedocs.io/en/latest/spec/), the other docs at [typing.readthedocs.io/en/latest](https://typing.readthedocs.io/en/latest/), or the docs at [docs.python.org/3/library/typing.html](https://docs.python.org/3/library/typing.html).
- Use more concise language in a few places

---

_Comment by @github-actions[bot] on 2024-03-20 17:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_pyi/rules/unrecognized_platform.rs`:43 on 2024-03-21 08:05_

nit: maybe just "Python Typing Documentation: Version and Platform Checks" ? Or "Typing Documentation: Version and Platform Checks"

---

_@dhruvmanila approved on 2024-03-21 08:06_

Reads well, thank you!

---

_Label `documentation` added by @dhruvmanila on 2024-03-21 08:06_

---

_Closed by @AlexWaygood on 2024-03-21 11:46_

---

_Reopened by @AlexWaygood on 2024-03-21 11:46_

---

_Merged by @AlexWaygood on 2024-03-21 11:54_

---

_Closed by @AlexWaygood on 2024-03-21 11:54_

---

_Branch deleted on 2024-03-21 11:54_

---
