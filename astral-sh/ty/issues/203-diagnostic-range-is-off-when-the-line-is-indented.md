```yaml
number: 203
title: diagnostic range is off when the line is indented with tabs
type: issue
state: closed
author: InSyncWithFoo
labels:
  - bug
  - diagnostics
assignees: []
created_at: 2025-02-06T01:20:28Z
updated_at: 2025-06-26T16:27:29Z
url: https://github.com/astral-sh/ty/issues/203
synced_at: 2026-01-12T15:54:22Z
```

# diagnostic range is off when the line is indented with tabs

---

_@InSyncWithFoo_

Given this file:

```python
def f():
	return (1 == '2')()  # Tab indented
```

Ruff gets the range correctly:

```python
$ ruff check --select PLR0133 --isolated
foo.py:2:10: PLR0133 Two constants compared in a comparison, consider replacing `1 == '2'`
  |
1 | def f():
2 |     return (1 == '2')()  # Tab indented
  |             ^ PLR0133
  |

Found 1 error.
```

But Red Knot doesn't:

```shell
$ red_knot check --venv-path .venv
error: lint:call-non-callable
 --> ~\project\foo.py:2:9
  |
1 | def f():
2 |     return (1 == '2')()  # Tab indented
  |        ^^^^^^^^^^^^ Object of type `bool` is not callable
  |
```

---

_Label `diagnostics` added by @dhruvmanila on 2025-02-06 04:03_

---

_Label `bug` added by @MichaReiser on 2025-02-06 06:45_

---

_Renamed from "Red Knot's diagnostic range is off when the line is indented with tabs" to "[red-knot] diagnostic range is off when the line is indented with tabs" by @carljm on 2025-03-27 18:59_

---

_Renamed from "[red-knot] diagnostic range is off when the line is indented with tabs" to "diagnostic range is off when the line is indented with tabs" by @MichaReiser on 2025-05-07 15:26_

---

_Comment by @AlexWaygood on 2025-06-26 15:21_

It appears that this has been fixed by https://github.com/astral-sh/ruff/pull/18962 🥳

@BurntSushi, do you think it's worth adding another regression test for this specifically, or would that just duplicate the tests you already added in that PR?

---

_Comment by @BurntSushi on 2025-06-26 16:24_

Nice find! Sounds like a good idea: https://github.com/astral-sh/ruff/pull/18965

---

_Closed by @BurntSushi on 2025-06-26 16:27_

---
