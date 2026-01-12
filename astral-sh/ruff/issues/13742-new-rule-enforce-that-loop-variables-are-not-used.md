```yaml
number: 13742
title: new rule - enforce that loop variables are not used outside the loop
type: issue
state: closed
author: DetachHead
labels: []
assignees: []
created_at: 2024-10-14T02:56:36Z
updated_at: 2024-10-14T04:33:05Z
url: https://github.com/astral-sh/ruff/issues/13742
synced_at: 2026-01-12T15:54:53Z
```

# new rule - enforce that loop variables are not used outside the loop

---

_@DetachHead_

the fact that loop variables are still in scope outside of the loop is a very common source of bugs:

```py
for val in values:
    print(val)

... # several lines later

value = 1
print(val) # it's very likely the user intended to use value instead of val
```



i know there are cases where this is intentional, for example:
```py
for value in values:
    ...
print(f"the last value was {value}")
```
but imo it's a bad idea to rely on this functionality. what if `values` was empty? `value` will be undefined which will cause a runtime error.

in most other languages, loop variables are only in scope within the loop, which is much less error-prone. it would be nice if there was a rule that enforced this:

```py
for val in values:
    ...
print(val) # error: loop variable used outside of loop
```

---

_Comment by @KotlinIsland on 2024-10-14 03:01_

> what if `values` was empty?

you would get an error in [bpr](https://basedpyright.com/?typeCheckingMode=all&code=CYUwZgBGAUBuCGAbAriAzgLgoglmgLgNo4B2%2BAugJQYCwAUBI1APYBOECKIEpHSqmek2EQAdOKFMADq1L5oYAET4AFt0TwCfLhADumiAG9OqAL6LKkxkA)

---

_Comment by @KotlinIsland on 2024-10-14 03:03_

i think the idea of inserting `del` after every loop would be highly tedious. perhaps a better rule would be "don't use loop var outside of loop"

---

_Renamed from "new rule - enforce that loop variables are deleted at the end of the loop" to "new rule - enforce that loop variables are not used outside the loop" by @DetachHead on 2024-10-14 03:05_

---

_Comment by @KotlinIsland on 2024-10-14 03:07_

could this also apply to if statements with walrus:
```py
if match := re.search(...):
  print(match)
print(match)  # error, don't do that
```



---

_Comment by @diceroll123 on 2024-10-14 04:27_

This would be Pylint's `W0631`, which at this time is marked as not implemented in Ruff, in #970 

https://pylint.readthedocs.io/en/stable/user_guide/messages/warning/undefined-loop-variable.html

---

_Comment by @DetachHead on 2024-10-14 04:33_

oh right, closing in favor of #970

---

_Closed by @DetachHead on 2024-10-14 04:33_

---
