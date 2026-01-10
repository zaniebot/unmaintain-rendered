---
number: 2192
title: "support \"tagged union\" narrowing via enum literal tags"
type: issue
state: open
author: carljm
labels:
  - narrowing
  - typeddict
assignees: []
created_at: 2025-12-23T19:42:07Z
updated_at: 2026-01-04T18:47:42Z
url: https://github.com/astral-sh/ty/issues/2192
synced_at: 2026-01-10T01:51:14Z
---

# support "tagged union" narrowing via enum literal tags

---

_Issue opened by @carljm on 2025-12-23 19:42_

We currently support "tagged union" narrowing of TypedDicts via string/bytes/int literals. We should also support fields whose value is an enum literal. (We need to watch out for enums that override `__eq__`.)

---

_Added to milestone `Stable` by @carljm on 2025-12-23 19:42_

---

_Label `narrowing` added by @carljm on 2025-12-23 19:42_

---

_Label `typeddict` added by @carljm on 2025-12-23 19:42_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-04 18:47_

---
