---
number: 9410
title: Implement more flake8-bugbear rules
type: issue
state: closed
author: mikaelarguedas
labels:
  - good first issue
  - rule
assignees: []
created_at: 2024-01-06T17:45:57Z
updated_at: 2024-01-06T22:41:03Z
url: https://github.com/astral-sh/ruff/issues/9410
synced_at: 2026-01-07T13:12:15-06:00
---

# Implement more flake8-bugbear rules

---

_Issue opened by @mikaelarguedas on 2024-01-06 17:45_

 (B035 aka RUF011 implemented in bugbear https://github.com/PyCQA/flake8-bugbear/issues/391)
- [ ] B035: Found dict comprehension with a static key - either a constant value or variable not from the comprehension expression. This will result in a dict with a single key that was repeatedly overwritten.

<details><summary>bugbear version's catches more problematic cases</summary>


```python
CONST_KEY_VAR = "KEY"

# bad
bad_const_key_var = {CONST_KEY_VAR: i for i in range(3)}

# bad - variabe not from generator
v3 = 1
bad_var_not_from_nested_tuple = {v3: k for k, (v1, v2) in {"a": (1, 2)}.items()}
```
RUF012 doesnt report any error

flake8-bugbear reports:
```
B035 Static key in dict comprehension 'CONST_KEY_VAR'.
B035 Static key in dict comprehension 'v3'.
```
</details>

- [ ] B036: Found except BaseException: without re-raising (no raise in the top-level of the except block). This catches all kinds of things (Exception, SystemExit, KeyboardInterrupt...) and may prevent a program from exiting as expected.


---

_Comment by @charliermarsh on 2024-01-06 19:42_

Nice, we can definitely catch those in `B035`.

---

_Label `good first issue` added by @charliermarsh on 2024-01-06 19:42_

---

_Label `rule` added by @charliermarsh on 2024-01-06 19:42_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-06 20:29_

---

_Comment by @charliermarsh on 2024-01-06 20:29_

I'm gonna do this for fun, and move the rule to `B035`.

---

_Referenced in [astral-sh/ruff#9411](../../astral-sh/ruff/pulls/9411.md) on 2024-01-06 20:35_

---

_Closed by @charliermarsh on 2024-01-06 20:44_

---

_Comment by @mikaelarguedas on 2024-01-06 20:56_

Awesome thanks!

2 newbies questions:
- Should this ticket stay opened for the implementation of B036 ?
- How does the moving / equaling error codes work ? is there a track somewhere of error codes that are equivalent ? (losely related to my reflection for https://github.com/astral-sh/ruff/issues/9354 where making equivalent rules visible could be a good first step)

---

_Comment by @charliermarsh on 2024-01-06 22:11_

@mikaelarguedas -- Ah yeah, I actually missed that second bullet. But I'm going to move that into #3758 and generalize the ticket since it's about a new rule.

We can rename rules and have them continue to work internally, so e.g. if folks have `RUF011` as a `# noqa` or similar it will continue to work with a redirect. We have some notes on rule aliasing here (https://github.com/astral-sh/ruff/issues/2186), but we're planning to do a big overhaul of the rule categories and hierarchies some time this year anyway.

---

_Comment by @mikaelarguedas on 2024-01-06 22:41_

> But I'm going to move that into https://github.com/astral-sh/ruff/issues/3758 and generalize the ticket since it's about a new rule.

:+1:

> We can rename rules and have them continue to work internally, so e.g. if folks have RUF011 as a # noqa or similar it will continue to work with a redirect.

Great! Does this appear on the website in the category of the relevant flake8 plugin ?

> We have some notes on rule aliasing here (https://github.com/astral-sh/ruff/issues/2186), but we're planning to do a big overhaul of the rule categories and hierarchies some time this year anyway.

:heart: 

---
