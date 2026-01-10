---
number: 4484
title: Expected for noqa to be ignored for multi-line string variable?
type: issue
state: closed
author: fennerm
labels:
  - bug
  - suppression
assignees: []
created_at: 2023-05-17T22:29:23Z
updated_at: 2023-05-18T13:05:29Z
url: https://github.com/astral-sh/ruff/issues/4484
synced_at: 2026-01-10T01:22:43Z
---

# Expected for noqa to be ignored for multi-line string variable?

---

_Issue opened by @fennerm on 2023-05-17 22:29_

(ruff=v0.0.267).

Based on the docs we can add a noqa annotation to a multi-line string as follows:
```
"""
Some long
string.
"""  # noqa
```

I would assume this should also be true if the string is a variable, but the noqa annotation seems to be ignored in that case.

Minimal example:
```
weirdly_Cased: str = """
Some long
string.
"""  # noqa: N816
```

Ruff outputs:
```
N816 Variable `weirdly_Cased` in global scope should not be mixedCase
RUF100 [*] Unused `noqa` directive (unused: `N816`)
```

Possible that this case is deliberately not supported yet but figured I'd flag it. Thanks!

---

_Renamed from "Is it expected for noqa to be ignored for this multi-line string variable?" to "Expected for noqa to be ignored for multi-line string variable?" by @fennerm on 2023-05-17 22:30_

---

_Comment by @charliermarsh on 2023-05-18 00:47_

Hmm -- I suspect this changed when we did the "byte offset" refactor (\cc @MichaReiser), though I'm torn on what should be considered "correct" here... The motivation for respecting `noqa` directives at the end of multi-line strings is that you can't feasible put a `noqa` directive _within_ a multi-line string (otherwise, it becomes part of the string itself). In this case, you _could_ put the `noqa` here:

```py
weirdly_Cased: str = (  # noqa: N816
    """
Some long
string.
"""
)
```

But, we're clearly not making that change with `--add-noqa`, and it feels somewhat "surprising" that the form you provided above doesn't work.

What do you think, @MichaReiser?


---

_Label `bug` added by @charliermarsh on 2023-05-18 00:47_

---

_Label `noqa` added by @charliermarsh on 2023-05-18 00:47_

---

_Comment by @MichaReiser on 2023-05-18 12:08_

I think this is a bug and can probably be fixed by remapping the range from the start of the line instead of the multiline-string range only in:

https://github.com/charliermarsh/ruff/blob/ddd541b1987493a69ef5e8f65ef8030eda6198cd/crates/ruff/src/directives.rs#L100-L107

by using `string_mappings.push(TextRange::new(locator.line_start(range.start()), range.end()));`

```py
weirdly_Cased: str = (  # noqa: N816
    """
Some long
string.
"""
)
```

should then continue to work because we only remap the lines of the multiline-string, not the assignment's line

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-18 12:54_

---

_Referenced in [astral-sh/ruff#4490](../../astral-sh/ruff/pulls/4490.md) on 2023-05-18 12:59_

---

_Closed by @charliermarsh on 2023-05-18 13:05_

---
