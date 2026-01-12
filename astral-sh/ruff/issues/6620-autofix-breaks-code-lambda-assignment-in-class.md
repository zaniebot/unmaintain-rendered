```yaml
number: 6620
title: "Autofix breaks code: lambda-assignment in class body"
type: issue
state: closed
author: Adamantish
labels:
  - bug
  - accepted
assignees: []
created_at: 2023-08-16T15:54:35Z
updated_at: 2023-08-17T02:07:55Z
url: https://github.com/astral-sh/ruff/issues/6620
synced_at: 2026-01-12T15:54:46Z
```

# Autofix breaks code: lambda-assignment in class body

---

_@Adamantish_

**Version**: 0.0.284
**Config**: No config. Just evaluating ruff.
**Command**: `ruff src --isolated --fix`

Codebase uses lambdas as values in an `Enum`. Whether that be considered a nice trick or not, it certainly doesn't work if those assignments are turned into methods.

```python
class TemperatureScales(Enum):
    CELSIUS = (lambda deg_c: deg_c)
    FAHRENHEIT = (lambda deg_c: deg_c * 9 / 5 + 32)

# will not work if converted to:

class TemperatureScales(Enum):
    def CELSIUS(deg_c):
        return deg_c
    def FAHRENHEIT(deg_c):
        return deg_c * 9 / 5 + 32
```




---

_Renamed from "Autofix breaks code: lambda-assignment" to "Autofix breaks code: lambda-assignment in class body" by @Adamantish on 2023-08-16 15:54_

---

_Label `bug` added by @charliermarsh on 2023-08-16 17:38_

---

_Label `accepted` added by @charliermarsh on 2023-08-16 17:38_

---

_Comment by @charliermarsh on 2023-08-16 17:38_

Thanks! It turns out that we already mark this as a manual-only fix when it's within a class. So it will automatically be avoided once we start respecting those annotations in the CLI.

---

_Comment by @charliermarsh on 2023-08-16 17:39_

Ah, we only do so for lambdas with annotations. I can change that.

---

_Comment by @charliermarsh on 2023-08-17 02:07_

Closing this optimistically as we've now marked these as "manual only" fixes. Once we respect those annotations in the CLI, we'll avoid these breakages in the future (though the violations themselves will still be raised, so either way I'd suggest a `# noqa` here). Thanks again for reporting!

---

_Closed by @charliermarsh on 2023-08-17 02:07_

---
