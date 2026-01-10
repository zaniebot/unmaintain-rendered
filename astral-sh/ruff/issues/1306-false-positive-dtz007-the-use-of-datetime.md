---
number: 1306
title: "False-positive `DTZ007`:  The use of `datetime.datetime.strptime()` without %z must be followed by `.replace(tzinfo=)`"
type: issue
state: open
author: actionless
labels:
  - wontfix
  - type-inference
assignees: []
created_at: 2022-12-20T22:09:56Z
updated_at: 2025-07-24T14:03:32Z
url: https://github.com/astral-sh/ruff/issues/1306
synced_at: 2026-01-10T01:22:39Z
---

# False-positive `DTZ007`:  The use of `datetime.datetime.strptime()` without %z must be followed by `.replace(tzinfo=)`

---

_Issue opened by @actionless on 2022-12-20 22:09_

```python
import datetime

DT_FORMAT = "%a, %d %b %Y %H:%M:%S %z"
file_data = "Fri, 23 Sep 2022 08:42:36 +0000"


parsed_date = datetime.datetime.strptime(
    file_data, DT_FORMAT
)
```

```console
$ ruff test_ruff_DTZ007.py
Found 1 error(s).
test_ruff_DTZ007.py:7:15: DTZ007 The use of `datetime.datetime.strptime()` without %z must be followed by `.replace(tzinfo=)`
```

---

_Comment by @charliermarsh on 2022-12-20 22:13_

Probably a stretch to fix this right now since it relies on static analysis that's beyond Ruff's current capabilities (understanding that `DT_FORMAT` is assigned to a string containing `%z`).

---

_Comment by @actionless on 2022-12-20 22:17_

yeah i was actually wondering how you going to address that [advanced static analysis] in the feature, by reusing parts of RustPython to actually run some parts?

---

_Label `wontfix` added by @charliermarsh on 2022-12-21 18:46_

---

_Comment by @andersk on 2022-12-26 02:00_

Just to dream a little bit here, I think there could be a future where we integrate with mypy or another type checker to enable type-aware lints.

In that world, one could write

```python
from typing import Final

DT_FORMAT: Final = "%a, %d %b %Y %H:%M:%S %z"
file_data = "Fri, 23 Sep 2022 08:42:36 +0000"

parsed_date = datetime.datetime.strptime(
    file_data, DT_FORMAT
)
```

The `Final` annotation ([PEP 591](https://peps.python.org/pep-0591/)) promises that the value won’t be reassigned, and allows the type checker to infer a type `Literal["%a, %d %b %Y %H:%M:%S %z"]` ([PEP 586](https://peps.python.org/pep-0586/)) for `DT_FORMAT`, which could be used in the Ruff rule.

This kind of integration would enable us to improve the accuracy of many rules and enable a variety of new ones.

---

_Comment by @charliermarsh on 2022-12-26 02:03_

As a starting point... we _could_ support `Final` specifically. Maybe that's slightly unprincipled though.

---

_Comment by @gdub on 2023-02-16 05:33_

A similar case I ran across today was that grabbing only date-related items from a string, e.g.:
```python
from datetime import datetime
datetime.strptime("2022-01-01", "%Y-%m-%d").date()
```
...also results in a `DTZ007` false-positive:
```
$ ruff check --isolated --select DTZ strptime_test.py 
strptime_test.py:2:1: DTZ007 The use of `datetime.datetime.strptime()` without %z must be followed by `.replace(tzinfo=)` or `.astimezone()`
Found 1 error.
```

In a case like this where not dealing with time, the resulting date value is the same and is not dependent on a timezone. To detect this case, though, it seems that we'd have to recognize both that the format string contains no time-related items _and_ that there's a following conversion with `.date()`.

---

_Label `type-inference` added by @charliermarsh on 2023-03-02 03:15_

---

_Comment by @youknowone on 2023-04-20 15:46_

@charliermarsh Does RustPython need to expose features out of core? `symboltable.rs` looks like related to this one. Not sure it is enough  though.

---

_Comment by @charliermarsh on 2023-04-21 02:15_

@youknowone - We do have some of our abstractions in Ruff for implementing a semantic model -- we track scopes, bindings, etc. I want to spend some time improving the semantic model, so I'll take a look at what you have in RustPython as part of that exercise -- maybe there are things we can leverage!

---

_Referenced in [astral-sh/ruff#7488](../../astral-sh/ruff/issues/7488.md) on 2023-09-18 10:14_

---

_Comment by @jonas-w on 2024-03-25 21:13_

Don't know if this is the same issue, but there is also a false-positive if the format string is an f-string.

For example:
```python
from datetime import datetime
def parse_iso(iso_str,millis=True):
    return datetime.strptime(iso_str, f"%Y-%m-%dT%H:%M:%S{('.%f' if millis else '')}%z")
```

Even without a condition inside the f-string, it immediately triggers the lint error.

For example:
```python
from datetime import datetime
dt = datetime.strptime("","%Y-%m-%d %H:%M:%S%z") # everything okay

dt = datetime.strptime("", f"%Y-%m-%d %H:%M:%S%z") # throws DTZ007
```

---

_Referenced in [astral-sh/ruff#10601](../../astral-sh/ruff/issues/10601.md) on 2024-03-26 07:45_

---

_Comment by @MichaReiser on 2024-03-26 07:46_

@jonas-w I think that's unrelated. I created a new issue to track the f-string format specifier handling.

---

_Comment by @nachocab on 2025-07-24 14:03_

I'm also getting a false positive when using the uppercase version `%Z` (Time zone name: UTC, GMT, etc.):

```python
from datetime import datetime

datetime.strptime('Thu, 28 Nov 2024 14:13:56 GMT', "%a, %d %b %Y %H:%M:%S %Z") # throws DTZ007
```

---
