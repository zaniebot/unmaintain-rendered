---
number: 16863
title: "How to check 'missing type annotation for function arguments' only for public functions?"
type: issue
state: open
author: Somya-Singhal
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-03-20T06:59:53Z
updated_at: 2025-03-24T14:32:54Z
url: https://github.com/astral-sh/ruff/issues/16863
synced_at: 2026-01-07T13:12:16-06:00
---

# How to check 'missing type annotation for function arguments' only for public functions?

---

_Issue opened by @Somya-Singhal on 2025-03-20 06:59_

### Question

Why does the 'flake8-annotations' rule ANN001 not distinguished between private, protected and public functions? For instance, there are separate error codes (ANN201, ANN202) for checking missing return type annotation for public and private functions.
Is there a way to check for missing type annotation in function arguments only for public functions?

### Version

ruff- 0.4.2

---

_Label `question` added by @Somya-Singhal on 2025-03-20 06:59_

---

_Comment by @ntBre on 2025-03-24 14:32_

That behavior appears to have been inherited from the upstream `flake8-annotations`, but we could consider extending the rule or the configuration.

---

_Label `question` removed by @ntBre on 2025-03-24 14:32_

---

_Label `rule` added by @ntBre on 2025-03-24 14:32_

---

_Label `needs-decision` added by @ntBre on 2025-03-24 14:32_

---
