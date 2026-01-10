---
number: 21993
title: "detect unnecessary keys() call in e.g. `for k in sorted(my_dict.keys()):`"
type: issue
state: open
author: ctcjab
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-12-15T22:34:40Z
updated_at: 2025-12-16T13:57:15Z
url: https://github.com/astral-sh/ruff/issues/21993
synced_at: 2026-01-10T01:23:02Z
---

# detect unnecessary keys() call in e.g. `for k in sorted(my_dict.keys()):`

---

_Issue opened by @ctcjab on 2025-12-15 22:34_

### Summary

Similar to https://docs.astral.sh/ruff/rules/in-dict-keys/ (which currently is not triggered when there is a surrounding call to `sorted`, `reversed`, etc.)

---

_Label `rule` added by @ntBre on 2025-12-16 13:53_

---

_Label `needs-decision` added by @ntBre on 2025-12-16 13:54_

---

_Comment by @ntBre on 2025-12-16 13:57_

This might make sense as an extension to SIM118 rather than a separate rule too.

---
