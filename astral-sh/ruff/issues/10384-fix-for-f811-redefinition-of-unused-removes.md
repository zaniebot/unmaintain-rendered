```yaml
number: 10384
title: Fix for F811 (Redefinition of unused) removes import needed at runtime
type: issue
state: closed
author: Hnasar
labels:
  - bug
  - rule
assignees: []
created_at: 2024-03-13T14:24:08Z
updated_at: 2024-03-18T13:32:37Z
url: https://github.com/astral-sh/ruff/issues/10384
synced_at: 2026-01-10T11:09:52Z
```

# Fix for F811 (Redefinition of unused) removes import needed at runtime

---

_Issue opened by @Hnasar on 2024-03-13 14:24_

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

If you have a module and a symbol of the same name, https://docs.astral.sh/ruff/rules/redefined-while-unused/ may result in broken code.
Using version: 0.3.2

## Repro
```py
import datetime
from datetime import datetime
datetime(1, 2, 3)
```
```console
$ ruff foo.py  --fix
```

## Expected

The first import (shadowed) should be removed.

## Observed

```diff
--- foo.py
+++ foo.py
@@ -2,6 +2,4 @@
 
 import datetime
 
-from datetime import datetime
-
 print(datetime(1, 2, 3))
```

```console
$ python3 foo.py 
Traceback (most recent call last):
  File "foo.py", line 5, in <module>
    datetime(1, 2, 3)
TypeError: 'module' object is not callable. Did you mean: 'datetime.datetime(...)'?
```


---

_Label `bug` added by @zanieb on 2024-03-13 14:29_

---

_Label `rule` added by @zanieb on 2024-03-13 14:29_

---

_Comment by @zanieb on 2024-03-13 14:29_

Thanks for the clear report! This one looks tricky.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-13 14:50_

---

_Comment by @charliermarsh on 2024-03-13 15:11_

I'll take a look!

---

_Comment by @MichaReiser on 2024-03-18 06:49_

@charliermarsh is this fixed by your PR or could you explain what's still missing.

---

_Comment by @charliermarsh on 2024-03-18 12:56_

Yeah it’s fixed.

---

_Closed by @charliermarsh on 2024-03-18 12:56_

---
