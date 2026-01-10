```yaml
number: 19696
title: "UP024 error message is confusing when `except` clause has multiple exceptions"
type: issue
state: open
author: burrscurr
labels:
  - rule
  - help wanted
assignees: []
created_at: 2025-08-01T20:10:24Z
updated_at: 2025-08-06T06:21:18Z
url: https://github.com/astral-sh/ruff/issues/19696
synced_at: 2026-01-10T11:09:59Z
```

# UP024 error message is confusing when `except` clause has multiple exceptions

---

_Issue opened by @burrscurr on 2025-08-01 20:10_

### Summary

[UP024](https://docs.astral.sh/ruff/rules/os-error-alias/) highlights specific aliased exceptions which might be removed in the future. When the `except` clause contains multiple exceptions, the error message can be a bit misleading. Simple Example:

```py
try:
    pass
except (IOError, ValueError):
    pass
```

In this example, `IOError` is an alias of `OSError`, that should be replaced, while `ValueError` is just a regular exception. Ruff correctly understands this (see `uv tool run ruff@0.12.7 check --select UP024 --fix --diff`), but its output does not distinguish between exceptions targeted by this rule and ones that aren't:

```
example.py:3:8: UP024 [*] Replace aliased errors with `OSError`
  |
1 | try:
2 |     pass
3 | except (ValueError, IOError):
  |        ^^^^^^^^^^^^^^^^^^^^^ UP024
4 |     pass
  |
  = help: Replace with builtin `OSError`
```

The error message "Replace aliased errors with `OSError`" in connection with the highlighted section makes it seem as if *both* exceptions were aliases of `OSError` and should be replaced.

If this is possible, specifically highlighting the aliased exceptions would probably be sufficient to avoid this misinterpretation:

```
example.py:3:8: UP024 [*] Replace aliased errors with `OSError`
  |
1 | try:
2 |     pass
3 | except (ValueError, IOError):
  |                     ^^^^^^^ UP024
4 |     pass
  |
  = help: Replace with builtin `OSError`
```

### Version

ruff 0.12.7 (c5ac99889 2025-07-29)

---

_Renamed from "UP024 error message is confusing when `except` clause has multiple errors" to "UP024 error message is confusing when `except` clause has multiple exceptions" by @burrscurr on 2025-08-01 20:10_

---

_Comment by @ntBre on 2025-08-05 13:26_

Good catch! Looks like we're flagging the whole tuple instead of the specific exception, which would be a good improvement, I think.

---

_Label `rule` added by @ntBre on 2025-08-05 13:26_

---

_Label `help wanted` added by @MichaReiser on 2025-08-05 13:59_

---

_Comment by @dylwil3 on 2025-08-05 15:03_

I think this is because multiple exception types can be aliases for `OSError` in the tuple but we don't want to raise separate diagnostics for them. The fix replaces all the different aliases with `OSError`. 

So actually this might be a good case for multi-range diagnostics once they go live - which I think is soon!

---

_Comment by @ntBre on 2025-08-05 15:58_

Ah okay, that makes sense. In some other rules I think we do raise multiple diagnostics and then clone the same fix into each of them, but a multi-span diagnostic could work too!

---

_Comment by @MichaReiser on 2025-08-06 06:21_

Yeah, I don't think we have an established principle for rules that may raise multiple diagnostics at the same line. Some rules only create one diagnostic, other rules create multiple diagnostics with the same fix attached. 

---
