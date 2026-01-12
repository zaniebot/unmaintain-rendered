```yaml
number: 11483
title: Clarify motivation for E713 and E714
type: pull_request
state: merged
author: mnme
labels:
  - documentation
assignees: []
merged: true
base: main
head: e713-e714-improve-description
created_at: 2024-05-21T17:43:38Z
updated_at: 2024-05-21T19:24:37Z
url: https://github.com/astral-sh/ruff/pull/11483
synced_at: 2026-01-12T15:55:38Z
```

# Clarify motivation for E713 and E714

---

_@mnme_

The wording 'negative comparison' is a rather vague description of the 'is not' operation and does not describe what the 'not in' operation does (potentially copied from 'is not'). This was replaced with more precise language to describe the operators taken from the official python docs[1].

Both rules didn't have a strong reasoning besides 'it's bad, use the other'. The origin of these rules seems to be PEP8[2] which prefers 'is not' over 'not ... is' for readability. This is now reflected in the description.

[1]: https://docs.python.org/3/reference/expressions.html#membership-test-operations
[2]: https://peps.python.org/pep-0008/#programming-recommendations

---

_Comment by @github-actions[bot] on 2024-05-21 17:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@zanieb approved on 2024-05-21 19:12_

Thank you!

---

_Label `documentation` added by @zanieb on 2024-05-21 19:12_

---

_Merged by @zanieb on 2024-05-21 19:12_

---

_Closed by @zanieb on 2024-05-21 19:12_

---

_Branch deleted on 2024-05-21 19:24_

---
