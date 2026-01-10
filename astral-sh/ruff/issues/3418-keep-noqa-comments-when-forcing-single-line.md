```yaml
number: 3418
title: "Keep `# noqa` comments when forcing single-line imports"
type: issue
state: closed
author: stinodego
labels:
  - bug
  - isort
assignees: []
created_at: 2023-03-09T13:41:25Z
updated_at: 2023-03-14T18:48:14Z
url: https://github.com/astral-sh/ruff/issues/3418
synced_at: 2026-01-10T11:09:46Z
```

# Keep `# noqa` comments when forcing single-line imports

---

_Issue opened by @stinodego on 2023-03-09 13:41_

Consider the following file `repro.py`:

```python
from datetime import date, time  # noqa: F401
```

And let's say my ruff config has `isort` lints, with force-single-line option:

```toml
[tool.ruff]
fix = true
select = ["F", "I"]

[tool.ruff.isort]
force-single-line = true
```

Running `ruff repro.py` results in the `time` import being deleted:

```python
from datetime import date  # noqa: F401
```

Running `ruff repro.py --unfixable=F401` results in:

```python
from datetime import date  # noqa: F401
from datetime import time
```

So the problem here is that the `# noqa` comment is not reproduced on the extra lines. Desired outcome would be:

```python
from datetime import date  # noqa: F401
from datetime import time  # noqa: F401
```

This is on the latest version of ruff (`0.0.254`).

---

_Label `bug` added by @charliermarsh on 2023-03-12 05:27_

---

_Label `isort` added by @charliermarsh on 2023-03-12 05:27_

---

_Comment by @charliermarsh on 2023-03-12 05:27_

(Do you know if `isort` does this?)

---

_Comment by @stinodego on 2023-03-12 15:05_

I checked, and `isort` keeps the comment on the first line of the split imports (same as `ruff` does currently). The other lines have no comments (goes for regular comments too).

I'd argue that this is a shortcoming in `isort`, but one that I can sort-of live with.
For `ruff`, thanks to its amazing autofix functionality, this is a more serious issue because it's deleting valid imports.

---

_Comment by @charliermarsh on 2023-03-12 17:46_

Yeah that makes sense. Do you think `# noqa` comments should be special-cased here? Or should this apply to any end-of-line comment on-split?

---

_Comment by @stinodego on 2023-03-12 18:33_

There are valid comments that one could like to see replicated:

```python
from datetime import date, time  # noqa: F401  # Import side effects required for functionality X
```

Without knowing what's in the comments exactly, I think our best guess is to just replicate the full comment entirely (noqa and regular).



---

_Comment by @Kilo59 on 2023-03-13 02:41_

This would make adopting new ruff rules or bringing new code under the coverage of `ruff` much easier.

The way this can be done now is by turning on some new rule and then using `--noqa` to add in-line ignores to any existing violations. However, on very large code bases this invariably leads to many lines that have to be formatted (via `black` or the `ruff` `I` rules). The newly formatted lines now no longer have an inline `# noqa` comment and require manual intervention.

Although this can be mitigated by running `ruff` 2 more times, once with `--noqa` (to add back `noqa` comments to the newly formatted lines) and then again with `--select RUF100 --fix` (to remove the now unused `noqa` comments).
 



---

_Closed by @charliermarsh on 2023-03-14 18:48_

---
