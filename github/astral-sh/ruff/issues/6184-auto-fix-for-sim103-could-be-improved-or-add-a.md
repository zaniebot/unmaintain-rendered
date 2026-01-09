---
number: 6184
title: Auto-fix for SIM103 could be improved or add a new SIM rule?
type: issue
state: open
author: klistwan
labels:
  - rule
  - help wanted
  - accepted
assignees: []
created_at: 2023-07-31T08:31:27Z
updated_at: 2025-03-31T08:09:35Z
url: https://github.com/astral-sh/ruff/issues/6184
synced_at: 2026-01-07T13:12:15-06:00
---

# Auto-fix for SIM103 could be improved or add a new SIM rule?

---

_Issue opened by @klistwan on 2023-07-31 08:31_

I came across this code on a project:

```python
if not condition_a and condition_b:
    return True
elif condition_a and condition_b:
    return True
else:
    return False
```

`condition_a` is redundant and this entire snippet should be simplified to:

```python
return bool(condition_b)
```

However, this is what ruff produced using `ruff --select="SIM103" example.py --fix --isolated`:

```python
if not condition_a and condition_b:
  return True
return bool(condition_a and condition_b)
```

I think this is correct reading the [rule docs](https://beta.ruff.rs/docs/rules/needless-bool/) but I thought there could be some improvement here either within this rule or a new rule.

Ruff version is 0.0.280 and config:

```toml
[tool.ruff]
line-length = 120
select = [
  "D3",  # pydocstyle
  "D40", # pydocstyle
  "DTZ", # flake8-datetimez
  "FLY", # flynt
  "I",   # isort
  "INT", # flake8-gettext
  "PIE", # flake8-pie
  "PLC", # pylint
  "PLE", # pylint
  "RSE", # flake8-raise
  "SIM2", # flake8-simplify
  "SIM3", # flake8-simplify
  "TCH", # flake8-type-checking
  "W",   # pycodestyle
  "YTT", # flake8-2020
]
target-version = "py37"
```

---

_Label `rule` added by @charliermarsh on 2023-08-04 02:53_

---

_Label `needs-decision` added by @charliermarsh on 2023-08-04 02:53_

---

_Comment by @dimbleby on 2024-06-01 15:43_

here's another example of an un-pleasing suggestion from SIM103.  I wavered over whether to raise this separately but decided is is related enough: easy to split things apart later if desired.
```python
def foo(num: int) -> bool:
    if not 10 < num < 100:
        return False

    return True
```
```
foo.py:2:5: SIM103 Return the condition `not not 10 < num < 100` directly
  |
1 |   def foo(num: int) -> bool:
2 |       if not 10 < num < 100:
  |  _____^
3 | |         return False
4 | |
5 | |     return True
  | |_______________^ SIM103
  |
  = help: Replace with `return not not 10 < num < 100`
```
`not not`? yuk!

---

_Comment by @charliermarsh on 2024-06-01 17:46_

These both look like things we can fix.

---

_Label `needs-decision` removed by @charliermarsh on 2024-06-01 17:46_

---

_Label `help wanted` added by @charliermarsh on 2024-06-01 17:46_

---

_Label `accepted` added by @charliermarsh on 2024-06-01 17:46_

---

_Referenced in [astral-sh/ruff#11684](../../astral-sh/ruff/pulls/11684.md) on 2024-06-01 23:14_

---

_Referenced in [astral-sh/ruff#11685](../../astral-sh/ruff/issues/11685.md) on 2024-06-01 23:16_

---

_Comment by @charliermarsh on 2024-06-01 23:22_

Fixed @dimbleby issue quickly in https://github.com/astral-sh/ruff/pull/11684.

---

_Comment by @tdulcet on 2025-01-08 21:10_

Here is another example of a double negative introduced by SIM103:
```diff
-	if "admin" not in privs: return False
-	return True
+	return not "admin" not in privs
```
and E713 (not-in-test) does not catch this. See the full example here: https://github.com/mail-in-a-box/mailinabox/pull/2473/commits/b2428eab153a78b2c3261a11f154133df43d17ad.

---

_Referenced in [astral-sh/ruff#15562](../../astral-sh/ruff/pulls/15562.md) on 2025-01-18 00:07_

---

_Comment by @spaceone on 2025-03-31 08:08_

For the following code, `SIM103` detects only the if-block last case.
```python
if foo:
   return True
if bar:
   return True
if baz:
    return True
return False
```
but in theory the whole block has that issue.
So a nicer fix would be:
`return bool(foo or bar or baz)`

---

_Comment by @spaceone on 2025-03-31 08:09_

```python
if not search():
    return False
return True
```

is transformed into:
`return search()`
but should be:
`return bool(search())`

---
