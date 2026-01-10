---
number: 13120
title: "Formatter: quote-style bug"
type: issue
state: closed
author: blackteahamburger
labels:
  - question
  - formatter
assignees: []
created_at: 2024-08-27T09:25:53Z
updated_at: 2024-08-28T11:07:45Z
url: https://github.com/astral-sh/ruff/issues/13120
synced_at: 2026-01-10T01:22:53Z
---

# Formatter: quote-style bug

---

_Issue opened by @blackteahamburger on 2024-08-27 09:25_

With `quote-style = "double"`.

Example:

```
# test.py
test = {"x": 0}
print(f"{test['x']}")
```
```
$ ruff format test.py
1 file left unchanged
```
`test['x']` is left unchanged.


---

_Label `formatter` added by @AlexWaygood on 2024-08-27 19:04_

---

_Comment by @AlexWaygood on 2024-08-27 19:06_

Hi! I can reproduce with `target-version` set to `"py311"`, but not with `target-version` set to `"py312"`. Do you have `target-version` or `requires-python` set in your `pyproject.toml` or `ruff.toml` config file at all? (The reason for this is that `f"{test["x"]})"` is not a valid f-string on Python 3.11. Python's grammar was changed in 3.12 as part of PEP 701.)

---

_Label `question` added by @AlexWaygood on 2024-08-27 19:06_

---

_Label `needs-info` added by @AlexWaygood on 2024-08-27 19:07_

---

_Comment by @blackteahamburger on 2024-08-28 02:26_

I can reproduce it with `ruff format test.py --config "target-version = 'py312'" --config "format.quote-style = 'double'"`. I'm using ruff 0.6.2.

---

_Comment by @dhruvmanila on 2024-08-28 04:53_

Is the expected output that the formatter should change the code like so?
```diff
- print(f"{test['x']}")
+ print(f"{test["x"]}")
```

That is available as a preview style, but only when the target version is 3.12 or later: https://play.ruff.rs/9cea74b1-c3fe-4f08-94bc-05f0c53a7ead

The reason is that f-string with repeated quotes is only valid for 3.12 or later and the formatting style is currently under preview: https://github.com/astral-sh/ruff/discussions/9785

---

_Label `needs-info` removed by @dhruvmanila on 2024-08-28 04:55_

---

_Comment by @blackteahamburger on 2024-08-28 10:23_

Yes, it's expected, thanks.



---

_Closed by @blackteahamburger on 2024-08-28 11:07_

---
