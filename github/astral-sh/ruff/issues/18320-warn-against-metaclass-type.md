---
number: 18320
title: "Warn against `metaclass=type`"
type: issue
state: closed
author: JelleZijlstra
labels:
  - rule
assignees: []
created_at: 2025-05-26T14:18:34Z
updated_at: 2025-05-28T07:22:45Z
url: https://github.com/astral-sh/ruff/issues/18320
synced_at: 2026-01-07T13:12:16-06:00
---

# Warn against `metaclass=type`

---

_Issue opened by @JelleZijlstra on 2025-05-26 14:18_

### Summary

`type` is the default metaclass and adding `metaclass=type` is usually redundant.

Example in python/typeshed#14150. This is similar to https://docs.astral.sh/ruff/rules/useless-object-inheritance/ about inheriting from `object`.

---

_Referenced in [python/typeshed#14150](../../python/typeshed/pulls/14150.md) on 2025-05-26 14:18_

---

_Comment by @chirizxc on 2025-05-26 17:55_

@MichaReiser can you assign this to me? 

---

_Comment by @chirizxc on 2025-05-26 17:59_

and what should the rule code be?

---

_Comment by @fennr on 2025-05-26 18:09_

There is already a rule https://docs.astral.sh/ruff/rules/useless-metaclass-type/
It seems to be worth adding a check in it.

---

_Comment by @JelleZijlstra on 2025-05-26 18:23_

@fennr That's a little different as it's about the Python 2 idiom with `__metaclass__`.

---

_Comment by @MichaReiser on 2025-05-26 19:16_

> and what should the rule code be?

You can added it using the next available `UP` code.

For why this rule

> Even though __prepare__ is not required, the default metaclass (‘type’) implements it, for the convenience of subclasses calling it via super().
> source https://peps.python.org/pep-3115/



---

_Label `rule` added by @MichaReiser on 2025-05-26 19:16_

---

_Assigned to @chirizxc by @MichaReiser on 2025-05-26 19:17_

---

_Referenced in [astral-sh/ruff#18334](../../astral-sh/ruff/pulls/18334.md) on 2025-05-27 16:10_

---

_Closed by @MichaReiser on 2025-05-28 07:22_

---
