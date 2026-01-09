---
number: 3597
title: Make the mccabe checker ignore the case of a single top-level elif chain or match statement
type: issue
state: open
author: saolof
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-03-18T11:04:13Z
updated_at: 2024-10-19T08:55:25Z
url: https://github.com/astral-sh/ruff/issues/3597
synced_at: 2026-01-07T13:12:14-06:00
---

# Make the mccabe checker ignore the case of a single top-level elif chain or match statement

---

_Issue opened by @saolof on 2023-03-18 11:04_

This is a somewhat common edge case where there shouldn't be a need to use noqa comments. Non-looping functions with a nesting depth of 1 should be excluded from the mccabe checker, or there should be a separate error code for this particular kind of pattern if you want to lint or ignore it separately, such as "elif chain exceeds mccabe complexity".

Incidentally, while discussing code complexity KPIs, being able to set a maximum allowed nesting depth would be nice.

---

_Comment by @antonagestam on 2023-03-18 16:30_

I'd like to work on this.

---

_Comment by @charliermarsh on 2023-03-19 03:26_

Confirming my understanding: are you describing something akin to a top-level switch statement? Like:

```py
if foo == "bar":
  return 1
elif foo == "baz:
  return 2:
else ...
```

---

_Label `question` added by @charliermarsh on 2023-03-19 03:26_

---

_Comment by @saolof on 2023-03-19 09:56_

Yes, exactly. There is a clear difference in complexity between a match statement with 10 cases, and a piece of code that has 3 nested for loops with if-clauses randomly distributed inside.

In the case where you are doing the switch based on type you can of course split it up with the functools.dispatch decorator or methods, and in the case where you just return a value you can use a dictionary, but apart from those cases, one big match with any nested behaviour split off into a separate function is usually simpler than any replacement.

---

_Comment by @antonagestam on 2023-03-19 10:24_

@saolof I agree, and I think it can sometimes be detrimental to try and break apart code like this. The result isn't likely to be easier for a human to reason about, and is likely to lower cohesion. Single-level structures are the exception to the rule, because they are already easy to reason about and follow.

---

_Assigned to @antonagestam by @MichaReiser on 2023-03-19 12:39_

---

_Comment by @antonagestam on 2023-04-04 09:02_

Sorry, I haven't had much time to spend on this. I might get to it at some point, but if someone else is keen to work on this, please go ahead.

---

_Unassigned @antonagestam by @charliermarsh on 2023-04-04 16:45_

---

_Referenced in [pymodbus-dev/pymodbus#1477](../../pymodbus-dev/pymodbus/issues/1477.md) on 2023-04-24 14:36_

---

_Label `rule` added by @charliermarsh on 2023-07-10 01:17_

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:17_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:17_

---
