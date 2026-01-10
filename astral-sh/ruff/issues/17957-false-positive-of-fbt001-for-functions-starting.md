```yaml
number: 17957
title: "False positive of FBT001 for functions starting with `set_` and taking a single boolean argument"
type: issue
state: closed
author: sassanh
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-05-08T16:19:47Z
updated_at: 2025-05-09T08:53:17Z
url: https://github.com/astral-sh/ruff/issues/17957
synced_at: 2026-01-10T11:09:58Z
```

# False positive of FBT001 for functions starting with `set_` and taking a single boolean argument

---

_Issue opened by @sassanh on 2025-05-08 16:19_

### Summary

It is mentioned in the documentation of FBT001:

> Dunder methods that define operators are exempt from this rule, as are setters and `@override` definitions.

I think other than property setters, functions with their name starting with `set_` and taking a single positional boolean should be exempted. Or at least functions starting with `set_is_`.

The value of FBT001 is in detecting when a boolean value is being used as a flag instead of an enum or similar patterns, it should be fine with booleans when they are being used as values.

---

_Label `rule` added by @ntBre on 2025-05-08 19:36_

---

_Label `needs-decision` added by @MichaReiser on 2025-05-09 06:14_

---

_Comment by @MichaReiser on 2025-05-09 06:15_

Related to https://github.com/astral-sh/ruff/issues/9497

---

_Comment by @sassanh on 2025-05-09 08:50_

@MichaReiser I didn’t see that issue, I think we can close this as a duplicate of #9497

---

_Closed by @MichaReiser on 2025-05-09 08:53_

---
