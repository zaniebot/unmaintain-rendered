```yaml
number: 2764
title: "Relative import autofix should respect `src`"
type: issue
state: closed
author: sbrugman
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-02-11T15:06:57Z
updated_at: 2023-02-14T22:25:00Z
url: https://github.com/astral-sh/ruff/issues/2764
synced_at: 2026-01-12T15:54:43Z
```

# Relative import autofix should respect `src`

---

_@sbrugman_

Thanks for this addition. It seems not to work for me in a src-layout, though. Coming here from the item in the changelog...

```toml
# pyproject.toml
[tool.ruff]
src = ["src", "tests"]
```

```console
$ tree src/
src/
├── purepythonmilter
│   ├── api
│   │   ├── application.py
[...]
│   ├── protocol
│   │   ├── commands.py
│   │   ├── definitions.py
[...]
```

```diff
$ ruff --diff src/purepythonmilter/api/application.py 
--- src/purepythonmilter/api/application.py
+++ src/purepythonmilter/api/application.py
@@ -10,9 +10,9 @@
 from typing import TYPE_CHECKING, Any, ClassVar
 
 import attrs
+from src.protocol import commands, definitions, responses
+from src.server import milterserver
 
-from ..protocol import commands, definitions, responses
-from ..server import milterserver
 from . import interfaces, logger, models
```

I expected `s/src/packagename/` here. Plus, it does not detect it as a first-party module here, I think, as it puts it with the third-party import `attrs`.

I would have expected it to change it into:

```diff
$ ruff --diff src/purepythonmilter/api/application.py 
--- src/purepythonmilter/api/application.py
+++ src/purepythonmilter/api/application.py
@@ -10,9 +10,9 @@
 from typing import TYPE_CHECKING, Any, ClassVar
 
 import attrs

+from purepythonmilter.protocol import commands, definitions, responses
+from purepythonmilter.server import milterserver 
-from ..protocol import commands, definitions, responses
-from ..server import milterserver
 from . import interfaces, logger, models
```

Am I doing something wrong or is this an omission? Thanks!

_Originally posted by @gertvdijk in https://github.com/charliermarsh/ruff/issues/2739#issuecomment-1426792627_
            

---

_Label `bug` added by @charliermarsh on 2023-02-11 17:00_

---

_Label `autofix` added by @charliermarsh on 2023-02-11 17:00_

---

_Comment by @charliermarsh on 2023-02-11 17:03_

@sbrugman - If you look at the `resolve_call_path` implementation, we actually sort of implement this already via `self.module_path`. Maybe can borrow that logic? (I should've caught this in review, my bad.)

---

_Comment by @sbrugman on 2023-02-13 18:46_

Thanks for the pointers, working on a fix.

---

_Closed by @charliermarsh on 2023-02-14 22:25_

---
