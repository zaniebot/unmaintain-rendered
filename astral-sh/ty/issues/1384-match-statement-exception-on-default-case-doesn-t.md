```yaml
number: 1384
title: "Match statement + exception on default case doesn't narrow properly"
type: issue
state: closed
author: drewbrew
labels: []
assignees: []
created_at: 2025-10-17T13:52:10Z
updated_at: 2025-10-17T16:00:09Z
url: https://github.com/astral-sh/ty/issues/1384
synced_at: 2026-01-12T15:54:25Z
```

# Match statement + exception on default case doesn't narrow properly

---

_@drewbrew_

### Summary

Example function:

```python
def do_something(interval: str, number_of_periods: int | None = None) -> None:
    match interval:
        case "monthly":
            if number_of_periods is None:
                number_of_periods = 12
            # do other stuff here
        case "daily":
            if number_of_periods is None:
                number_of_periods = 30
        case "hourly":
            if number_of_periods is None:
                number_of_periods = 24
        case _:
            raise ValueError(f"Unknown interval {interval}")
    if number_of_periods < 0 or number_of_periods > 50:
        raise ValueError(f"Invalid number of periods {number_of_periods}")
    # now fetch some data or something, IDK
```

Running `ty check` on this function returns two errors because ty does not catch that `number_of_periods` has to be an int at that point (each branch of the match either coerces that variable into an int or fails out with an exception).

```
error[unsupported-operator]: Operator `<` is not supported for types `None` and `int`, in comparing `int | None` with `Literal[0]`
  --> ty_fail.py:15:8
   |
13 |         case _:
14 |             raise ValueError(f"Unknown interval {interval}")
15 |     if number_of_periods < 0 or number_of_periods > 50:
   |        ^^^^^^^^^^^^^^^^^^^^^
16 |         raise ValueError(f"Invalid number of periods {number_of_periods}")
17 |     # now fetch some data or something, IDK
   |
info: rule `unsupported-operator` is enabled by default

error[unsupported-operator]: Operator `>` is not supported for types `None` and `int`, in comparing `int | None` with `Literal[50]`
  --> ty_fail.py:15:33
   |
13 |         case _:
14 |             raise ValueError(f"Unknown interval {interval}")
15 |     if number_of_periods < 0 or number_of_periods > 50:
   |                                 ^^^^^^^^^^^^^^^^^^^^^^
16 |         raise ValueError(f"Invalid number of periods {number_of_periods}")
17 |     # now fetch some data or something, IDK
   |
info: rule `unsupported-operator` is enabled by default
```

This _may_ be related to #1349 but I'm not 100% sure.

### Version

ty 0.0.1-alpha.23

---

_Comment by @sharkdp on 2025-10-17 13:58_

Thank you for reporting this. It looks like this is an instance of #690.

---

_Closed by @sharkdp on 2025-10-17 13:58_

---

_Comment by @drewbrew on 2025-10-17 16:00_

Yep, that indeed looks like the right duplicate. Thanks, @sharkdp!

---
