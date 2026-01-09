---
number: 12093
title: UP036 multiple conditions false negative
type: issue
state: open
author: gamer191
labels:
  - rule
assignees: []
created_at: 2024-06-28T14:18:59Z
updated_at: 2025-03-04T10:19:45Z
url: https://github.com/astral-sh/ruff/issues/12093
synced_at: 2026-01-07T13:12:15-06:00
---

# UP036 multiple conditions false negative

---

_Issue opened by @gamer191 on 2024-06-28 14:18_

UP036 doesn't work if there are multiple conditions. For a file Test2.txt with the text:
```
import sys
if True and sys.version_info < (3, 5):
    print("35")
```
running `ruff check Test2.txt --extend-select UP --config "target-version = 'py38'" --isolated` will result in `All checks passed!`
```
>ruff --version
ruff 0.5.0

>cat Test2.txt
import sys
if True and sys.version_info < (3, 5):
    print("35")


>ruff check Test2.txt --extend-select UP --config "target-version = 'py38'" --isolated
All checks passed!

```

---

_Comment by @dhruvmanila on 2024-07-01 04:43_

It seems reasonable to check for conditions although that means that we'd need to get a range of version if there are multiple `sys.version_info` checks like:
```py
if sys.version_info > (3, 5) and sys.version_info < (3, 10): ...
```
Here, we might want to raise `UP036` for `(3, 5)` and could restrict it to the `target-version`. I'm curious to hear others thoughts on this.

---

_Label `rule` added by @dhruvmanila on 2024-07-01 04:44_

---

_Comment by @gamer191 on 2024-07-06 12:01_

> if sys.version_info > (3, 5) and sys.version_info < (3, 10): ...

I think both should be considered individually. `sys.version_info > (3, 5)` is not allowed unless the target version is set to 3.5 (even then, it's not currently allowed due to #8832), so that should raise an error. `sys.version_info < (3, 10)` should raise another error if the target version is 3.10 or above (I don't know if raising multiple identical errors for one line is possible)

The same should obviously apply for:
```
if sys.version_info < (3, 5) or sys.version_info > (3, 10):
```




Real world example (which I've reported to the yt-dlp devs): https://github.com/yt-dlp/yt-dlp/blob/4862a29854d4044120e3f97b52199711ad04bee1/yt_dlp/compat/__init__.py#L39

---

_Comment by @Avasam on 2025-03-04 00:16_

I agree with @gamer191 here. Each condition should be considered individually. It's fine if you stop attempting to provide an autofix as soon as there's multiple conditions. But I would expect Ruff to flag `UP036` here.

https://github.com/astral-sh/ruff/issues/16487 is a very similar request (except that I extend it to any version comparison, instead of only in conditional)

---

_Referenced in [astral-sh/ruff#16487](../../astral-sh/ruff/issues/16487.md) on 2025-03-04 10:41_

---
