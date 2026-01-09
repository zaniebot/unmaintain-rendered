---
number: 17311
title: "PLR1730 \"safe\" fix may delete comments"
type: issue
state: closed
author: JimMajor
labels:
  - fixes
  - help wanted
assignees: []
created_at: 2025-04-09T12:42:04Z
updated_at: 2025-04-18T16:49:03Z
url: https://github.com/astral-sh/ruff/issues/17311
synced_at: 2026-01-07T13:12:16-06:00
---

# PLR1730 "safe" fix may delete comments

---

_Issue opened by @JimMajor on 2025-04-09 12:42_

### Summary

When ruff with rule PLR1730 and --fix option is used, it will delete comments which were present in the if block reformatted to min/max functions.

For example:
```python
if some_value > LIMIT:
    # very important comment
    some_value = LIMIT
```
will turn into:
```python
some_value = min(some_value, LIMIT)
```
This works like that for any number of comment lines between `if` line and assignment line.

Other possibly connected issue is that, if we replace comment with docstring, the ruff rule PLR1730 will not be triggered at all.
That is:
```python
if some_value > LIMIT:
    """
    Strange place for a docstring
    """
    some_value = LIMIT
```
will pass without any notice.

### Version

0.11

---

_Comment by @MichaReiser on 2025-04-09 13:33_

The first is a bug, thanks for reporting. 

The second isn't, because that's not a valid docstring position. It's an expression statement with a useless string literal expression 

---

_Label `fixes` added by @MichaReiser on 2025-04-09 13:33_

---

_Label `help wanted` added by @MichaReiser on 2025-04-09 13:33_

---

_Comment by @VascoSch92 on 2025-04-09 18:23_

So if we check that there is a comment in the `if-statement`, then the fix should be unsafe: correct?

If yes, i can give a try ;)

---

_Comment by @ntBre on 2025-04-09 18:26_

> So if we check that there is a comment in the `if-statement`, then the fix should be unsafe: correct?

Yes, I think that's right.

> If yes, i can give a try ;)

I'll assign you! Please also add a `## Fix safety` section to the rule docs :)

---

_Assigned to @VascoSch92 by @ntBre on 2025-04-09 18:27_

---

_Referenced in [astral-sh/ruff#17459](../../astral-sh/ruff/pulls/17459.md) on 2025-04-18 12:51_

---

_Closed by @ntBre on 2025-04-18 16:49_

---
