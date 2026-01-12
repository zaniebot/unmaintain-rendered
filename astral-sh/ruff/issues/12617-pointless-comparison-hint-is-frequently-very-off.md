```yaml
number: 12617
title: pointless comparison hint is frequently very off base
type: issue
state: closed
author: lolbinarycat
labels:
  - good first issue
  - rule
  - help wanted
assignees: []
created_at: 2024-08-01T20:57:34Z
updated_at: 2024-09-03T05:21:50Z
url: https://github.com/astral-sh/ruff/issues/12617
synced_at: 2026-01-12T15:54:52Z
```

# pointless comparison hint is frequently very off base

---

_@lolbinarycat_

code [(playground)](https://play.ruff.rs/0a603c34-c21e-4ab2-b645-54116e8fcdd0):
```python
def is_some(n):
    n is not None
```

here, the user is trying to do an implicit return, but python does not have implicit returns.  the hint given is this:

```
B015: Pointless comparison. Did you mean to assign a value? Otherwise, prepend `assert` or remove it.
```

the chance of someone meaning to write `=` and instead writing `is not` is approximately zero.  if a pointless comparison appears at the end of a function, it should suggest returning it instead of suggesting `assert`, and the assignment suggestion should only appear if the operator contains `=`.

---

_Label `rule` added by @MichaReiser on 2024-08-01 21:04_

---

_Label `help wanted` added by @MichaReiser on 2024-08-01 21:04_

---

_Label `good first issue` added by @MichaReiser on 2024-08-01 21:04_

---

_Comment by @MichaReiser on 2024-08-01 21:05_

Makes sense to me. We can probably improve the message over all. 

---

_Comment by @AlexWaygood on 2024-08-02 11:11_

> the chance of someone meaning to write `=` and instead writing `is not` is approximately zero.

FWIW, I don't think that's the assumption the rule is making when it says "Did you mean to assign a value?". Instead, I think the rule is assuming that the user meant to write something like

```py
def is_some(n):
    x = n is not None
```

or

```py
GLOBAL_OBJECT = ...

def is_some(n):
    GLOBAL_OBJECT.x = n is not None
```

As a general rule, to me it seems more likely that someone made a typo than that they misunderstood the fundamentals of Python semantics (thinking that Python has implicit returns when it does not). But I think you're right that if it's the last line of the function, the more likely typo is that they forgot to use a `return` statement; it's less likely that they wanted to assign the value to something if it's the last line of a function.

TL;DR: I'd also accept a PR making the error message adaptive here depending on whether it's the last line of a function or not 😄

---

_Comment by @lolbinarycat on 2024-08-02 15:22_

> As a general rule, to me it seems more likely that someone made a typo than that they misunderstood the fundamentals of Python semantics (thinking that Python has implicit returns when it does not).

missing the entire left-hand side is a pretty big typo, and python is very frequently used by beginner programmers and programmers who do not usually specialize in python (like me, i'm mostly a rust programmer, but i know just enough python to get by with basic tweaks)

---

_Comment by @thejchap on 2024-09-02 17:36_

@MichaReiser @AlexWaygood looks like this one is up for grabs - would like to give it a shot (first time contributor here :)

---

_Comment by @thejchap on 2024-09-02 21:55_

nevermind - missed [this](https://github.com/astral-sh/ruff/pull/12944)

---

_Comment by @dhruvmanila on 2024-09-03 05:21_

Resolved in https://github.com/astral-sh/ruff/pull/12944. Sorry for the churn, @thejchap.

---

_Closed by @dhruvmanila on 2024-09-03 05:21_

---
