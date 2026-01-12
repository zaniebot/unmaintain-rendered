```yaml
number: 18365
title: "[Infinite loop] Conflict between F401 and PLC0414; redundant alias is removed, redundant alias is added as re-export"
type: issue
state: closed
author: GideonBear
labels:
  - bug
assignees: []
created_at: 2025-05-29T10:14:55Z
updated_at: 2025-06-20T18:20:28Z
url: https://github.com/astral-sh/ruff/issues/18365
synced_at: 2026-01-12T15:54:56Z
```

# [Infinite loop] Conflict between F401 and PLC0414; redundant alias is removed, redundant alias is added as re-export

---

_@GideonBear_

Ref #14283

`pyproject.toml`:
```toml
[tool.ruff]
lint.select = [ "F401", "PLC0414" ]
lint.preview = true
```
`package/__init__.py`:
```py
from package.module import function
```
`package/module.py`:
```py
def function(): ...
```
Run this command: `ruff check --unsafe-fixes --fix`
Result:
```console
$ ruff check --unsafe-fixes --fix

error: Failed to converge after 100 iterations.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `package/__init__.py`, the rule codes F401, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

package/__init__.py:1:28: F401 `package.module.function` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
  |
1 | from package.module import function
  |                            ^^^^^^^^ F401
  |
  = help: Use an explicit re-export: `function as function`

Found 101 errors (100 fixed, 1 remaining).
[*] 1 fixable with the --fix option.
```

---

_Renamed from "[Infinite loop] Conflict between F401 and PLC0414" to "[Infinite loop] Conflict between F401 and PLC0414; redundant alias is removed, redundant alias is added as re-export" by @GideonBear on 2025-05-29 10:16_

---

_Label `bug` added by @ntBre on 2025-05-29 13:01_

---

_Comment by @ntBre on 2025-05-29 13:09_

Thanks for the report and the great example! I can reproduce this.

I'm not sure about the best way to fix this, but one option could be some kind of special handling for `__init__.py` files in PLC0414. I know we have that for F401. We should probably just avoid offering a fix that would conflict with F401 in `__init__.py` files.

---

_Comment by @GideonBear on 2025-05-29 13:14_


> I'm not sure about the best way to fix this, but one option could be some kind of special handling for `__init__.py` files in PLC0414. I know we have that for F401. We should probably just avoid offering a fix that would conflict with F401 in `__init__.py` files.

Is it also acceptable to just disable PLC0414 in `__init__.py` files entirely? Seeing as there's an exception for F401 already, I don't think it makes sense to warn for redundant aliases there.

---

_Comment by @GideonBear on 2025-05-29 13:15_

Or is PLC0414 just in general incompatible with the idea of using redundant aliases?

---

_Comment by @ntBre on 2025-05-29 13:21_

Oh you're right, we probably shouldn't offer a diagnostic at all in `__init__.py`, not just avoid a fix.

And yeah, I think [PLC0414](https://docs.astral.sh/ruff/rules/useless-import-alias/) is a pretty straightforward rule currently, it forbids any redundant alias even if removing it would change the program's behavior. That's why the fix is unsafe:

> This fix is marked as always unsafe because the user may be intentionally re-exporting the import. While statements like import numpy as numpy appear redundant, they can have semantic meaning in certain contexts.

---

_Comment by @GideonBear on 2025-05-30 11:24_

I'm working on this

---

_Assigned to @GideonBear by @ntBre on 2025-05-30 11:54_

---

_Comment by @injust on 2025-06-03 22:14_

Related: https://github.com/astral-sh/ruff/issues/6294

---

_Comment by @Avasam on 2025-06-04 03:25_

I've been ignoring this rule in `__init__.py`: https://github.com/astral-sh/ruff/issues/6294#issuecomment-2018779663

---

_Comment by @GideonBear on 2025-06-04 05:20_

If I'm not mistaken, that means that #18400 closes #6294

---

_Closed by @dylwil3 on 2025-06-20 18:20_

---
