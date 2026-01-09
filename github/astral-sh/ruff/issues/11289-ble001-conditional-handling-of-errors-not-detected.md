---
number: 11289
title: "BLE001: conditional handling of errors not detected"
type: issue
state: closed
author: bentheiii
labels:
  - bug
assignees: []
created_at: 2024-05-05T09:38:18Z
updated_at: 2024-05-12T02:06:29Z
url: https://github.com/astral-sh/ruff/issues/11289
synced_at: 2026-01-07T13:12:15-06:00
---

# BLE001: conditional handling of errors not detected

---

_Issue opened by @bentheiii on 2024-05-05 09:38_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
keywords: BLE001, log, exception

The following code snippet does not trigger the BLE001 rule:
```py
from logging import exception


try:
    pass
except Exception:
    exception("An error occurred")
```

but this one does:
```py
from logging import exception

try:
    pass
except Exception:
    if True:
        exception("An error occurred")
    else:
        exception("An error occurred")
```

This is because ruff doesn't seem to consider the case the both branches of an `if` handle the error.

The reason we run into this is because sometimes we use code like the following:
```py
from logging import exception

try:
    pass
except Exception as ex:
    error_data = gather_diagnostic_data(...)
    if isinstance(ex, SomespecificError):
        exception("A specific error occurred", extra=error_data)
    else:
        exception("A generic error occurred", extra=error_data)
```

---

_Comment by @JonathanPlasse on 2024-05-05 11:33_

- Something similar was discussed previously in https://github.com/astral-sh/ruff/issues/1119#issuecomment-1341147418.

---

_Label `bug` added by @charliermarsh on 2024-05-06 01:21_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-06 01:21_

---

_Referenced in [astral-sh/ruff#11301](../../astral-sh/ruff/pulls/11301.md) on 2024-05-06 01:37_

---

_Closed by @charliermarsh on 2024-05-06 01:52_

---

_Comment by @rahulssheth on 2024-05-10 20:30_

We were recently investigating the 0.4.4 upgrade and found that snippets like this:

```
def method() -> None:
    try:
           raise Exception()
    except Exception:
           if False:
               raise
           else:
                  return None
```

would no longer raise `BLE001` (Ruff playground for reference: https://play.ruff.rs/f65629f5-eda0-43f0-a85f-c05a0be1cb9c) because there's a conditional handling the exception. This seems a bit problematic, given that the original issue was about cases where both (or rather, all) branches of a conditional handle the exception, but that isn't the case here. Totally understandable if this is out of scope, just wanted to put it on the radar. Thanks (apologies if this should be filed in another issue)!

---

_Comment by @JonathanPlasse on 2024-05-10 20:53_

A more complex visitor would need to be implemented like in flake8-trio.
https://github.com/astral-sh/ruff/issues/1119#issuecomment-1341171085

---

_Comment by @charliermarsh on 2024-05-10 20:54_

Thanks @rahulssheth. My overall feeling was that if you're re-raising even within a branch, then the use of the catch-all exception was probably intentional, and so likely to be a false positive. Are you seeing real cases where you _do_ want it to be flagged, despite a re-raise?

---

_Comment by @rahulssheth on 2024-05-12 02:05_

@charliermarsh good point! In most scenarios, if there's some sort of explicit handling with a re-raise, even if there isn't _always_ a re-raise, it likely isn't an problem. I mostly flagged this issue because I think there's a case to be made that false positives with an ignore should be preferred to false negatives here, but I haven't seen any real cases that merit a change of approach. Appreciate the quick feedback!

---
