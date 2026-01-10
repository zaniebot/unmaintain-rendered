---
number: 14637
title: Disable TC001, TC002, TC003 by default ?
type: issue
state: closed
author: RomainBrault
labels:
  - question
assignees: []
created_at: 2024-11-27T16:07:11Z
updated_at: 2024-11-27T17:36:13Z
url: https://github.com/astral-sh/ruff/issues/14637
synced_at: 2026-01-10T01:22:55Z
---

# Disable TC001, TC002, TC003 by default ?

---

_Issue opened by @RomainBrault on 2024-11-27 16:07_

It breaks runtime type checking.

See the discussions: https://github.com/beartype/beartype/discussions/460
And: https://github.com/beartype/beartype/pull/440#issuecomment-2421528743

It also break library that use type information at runtime.

From flake8-type-checking itself, see: https://github.com/snok/flake8-type-checking/issues/52#issuecomment-1004842907

Also `from __future__ import annotations` will be removed when Python 3.13 support will end. Moreover PEP 649, implemented in python 3.14, should solve the time penalty that TC001,2,3 try to resolve.

Alternative solutions:

disable starting from Python 3.14 only. Or do exception for all library that use runtime type information as in flake8 (e.g. beartype)

---

_Renamed from "Disable TC001, TC002, TC003" to "Disable TC001, TC002, TC003 by default" by @RomainBrault on 2024-11-27 16:07_

---

_Comment by @MichaReiser on 2024-11-27 16:38_

Hi @RomainBrault 

The `TC` rules are all disabled by default. You have to opt-in to them manually. It's possible that you did that by using the `ALL` selector but I'm not sure what Ruff should do different in that case. It would be very confusing if  `ALL` doesn't select *all* rules

---

_Label `question` added by @MichaReiser on 2024-11-27 16:38_

---

_Comment by @RomainBrault on 2024-11-27 17:20_

Indeed, i used the ALL selector 😳 , thanks for pointing it out

however it would be nice to point out that `from __future__ import annotations` will be deprecated in Python 3.14 (thus not using the rule is encouraged for the version >= 3.14) and the rule should be removed when 3.13 support will end.

---

_Renamed from "Disable TC001, TC002, TC003 by default" to "Disable TC001, TC002, TC003 by default ?" by @RomainBrault on 2024-11-27 17:26_

---

_Comment by @MichaReiser on 2024-11-27 17:27_

Thanks for pointing that out. We'll revisit all rules when Python 3.14 is closer to reaching beta status, and I'll close this as there's nothing immediate that needs to be done now. 

---

_Closed by @MichaReiser on 2024-11-27 17:27_

---
