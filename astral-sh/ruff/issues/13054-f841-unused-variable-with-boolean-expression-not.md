---
number: 13054
title: "F841: Unused variable with boolean expression not entirely deleted by --fix --unsafe-fixes"
type: issue
state: closed
author: Liritt
labels:
  - question
assignees: []
created_at: 2024-08-22T12:19:08Z
updated_at: 2024-08-22T13:22:04Z
url: https://github.com/astral-sh/ruff/issues/13054
synced_at: 2026-01-10T01:22:53Z
---

# F841: Unused variable with boolean expression not entirely deleted by --fix --unsafe-fixes

---

_Issue opened by @Liritt on 2024-08-22 12:19_

Hello,

We encountered an issue while using the --unsafe-fixes parameter with Ruff 0.6.1. Specifically, the removal of an unused variable containing a boolean expression caused a problem.

The problematic line is: `active = offer["status"] == "Active"`.
After running the command ruff check --fix --unsafe-fixes, Ruff only removed the `active =` portion, leaving the `offer["status"] == "Active"` part intact in our code.

Firstly, if Ruff can't delete the entire line, shouldn't it consider the line unfixable and ignore it?
Secondly, given that the variable is unused, perhaps Ruff should delete the entire line to avoid this kind of error? (suggestion)

Could you please address this issue?

Thanks!

---

_Renamed from "F841: Unused variable with boolean comparison not entirely deleted by --fix --unsafe-fixes" to "F841: Unused variable with boolean expressiob not entirely deleted by --fix --unsafe-fixes" by @Liritt on 2024-08-22 12:19_

---

_Renamed from "F841: Unused variable with boolean expressiob not entirely deleted by --fix --unsafe-fixes" to "F841: Unused variable with boolean expression not entirely deleted by --fix --unsafe-fixes" by @Liritt on 2024-08-22 12:19_

---

_Label `question` added by @MichaReiser on 2024-08-22 13:03_

---

_Comment by @MichaReiser on 2024-08-22 13:05_

Hi @Liritt 

I can see how removing the entire line is desired in this case. The reason why the rule's auto fix doesn't remove the entire line is because the right hand side could have a side effect in which case removing it would not be safe:

```python
def f():
	unused = sys.stdout.write("bla")
```

The good news is that you can use `B015` to detect and remove unused comparisons. Unfortunately, that rule has no auto fix today

---

_Comment by @Liritt on 2024-08-22 13:10_

I see, can we hope to see a feature adding an auto fix for this case in the near future or not ? Like a setting that would allow the user to choose whether or not they want to remove the unused comparisons ? Thanks

---

_Comment by @MichaReiser on 2024-08-22 13:18_

A future Ruff version may be able to remove the right-hand side if it can reason about whether the right-hand side has side effects or not.  I don't think we should introduce an option that always removes the right hand side because that can very easily lead to bugs.

---

_Comment by @Liritt on 2024-08-22 13:20_

I see. Well, thanks for replying ! We'll handle it by hand then.
Have a nice day !

---

_Closed by @Liritt on 2024-08-22 13:20_

---

_Comment by @MichaReiser on 2024-08-22 13:22_

Thanks, you too and sorry that we don't have the infrastructure today to handle this better.

---
