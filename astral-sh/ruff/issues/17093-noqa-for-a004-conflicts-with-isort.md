```yaml
number: 17093
title: "noqa for `A004`  conflicts with isort"
type: issue
state: open
author: spaceone
labels:
  - incompatibility
assignees: []
created_at: 2025-03-31T12:36:19Z
updated_at: 2025-07-29T17:04:24Z
url: https://github.com/astral-sh/ruff/issues/17093
synced_at: 2026-01-10T11:09:58Z
```

# noqa for `A004`  conflicts with isort

---

_Issue opened by @spaceone on 2025-03-31 12:36_

### Summary

ruff adds noqa to:
```python
from verylongline import (
    MODULE, Critical, Instance, ProblemFixed, Warning, util,  # noqa: A004
)
```

and `isort` moves it to:
```python
from verylongline import (  # noqa: A004
    MODULE, Critical, Instance, ProblemFixed, Warning, util,
)
```
which causes that `ruff` again detects it as error `A004`.

### Version

_No response_

---

_Comment by @MichaReiser on 2025-03-31 12:42_

isort moves the comment to a slightly different position for me

```py
from verylongling import (
    MODULE,  # noqa: A004
    Critical,
    Warning,
    util,
)

```

which then requires moving the `noqa` comment to the correct line.

I'm not sure how we can fix this and the manual "intervention" to move the comment to the right line doesn't seem too bad

Playground: https://play.ruff.rs/005cb17c-79b4-4a6f-aa25-16c2d502ac13


---

_Label `incompatibility` added by @MichaReiser on 2025-03-31 12:42_

---

_Comment by @spaceone on 2025-03-31 12:48_

probably depends on the isort config, mine is:
```ini
[tool.isort]
py_version = "37"
combine_as_imports = true
filter_files = true
force_grid_wrap = false
line_length = 120 
multi_line_output = 5 
include_trailing_comma = true
lines_after_imports = 2 
```

My workaround now was to add `# ruff: noqa: A004` to the whole file.

---

_Comment by @MichaReiser on 2025-03-31 12:50_

Oh, you use the `isort` tool, and not Ruff's `isort` implementation. We don't guarantee compatibility with other tools (as it's out of our control how isort moves the comment)

---

_Comment by @spaceone on 2025-03-31 12:59_

jeah, I can't use ruffs isort because of missing `multi_line_output = 5` compatibility.

---

_Comment by @Avasam on 2025-03-31 18:10_

I know it's just a workaround. But if you have the same symbols often creating this issue. You could try ignoring them: https://docs.astral.sh/ruff/settings/#lint_flake8-builtins_ignorelist

---
