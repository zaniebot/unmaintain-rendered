```yaml
number: 2739
title: autofix relative imports (TID252)
type: pull_request
state: merged
author: sbrugman
labels:
  - fixes
assignees: []
merged: true
base: main
head: autofix-relative-imports
created_at: 2023-02-10T20:22:39Z
updated_at: 2023-02-11T15:01:45Z
url: https://github.com/astral-sh/ruff/pull/2739
synced_at: 2026-01-12T04:52:00Z
```

# autofix relative imports (TID252)

---

_Pull request opened by @sbrugman on 2023-02-10 20:22_

autofix relative imports (TID252)

I found it unintuitive that a high level relative import (e.g. `.........parent`) resolves to directories above the project. We could build in a protection against it. VS Code does not have such a protection.

---

_Marked ready for review by @sbrugman on 2023-02-10 21:56_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_tidy_imports/relative_imports.rs`:54 on 2023-02-10 22:30_

Can we find a home for this in `ruff_python`?

---

_@charliermarsh reviewed on 2023-02-10 22:30_

---

_@charliermarsh reviewed on 2023-02-10 22:30_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_tidy_imports/relative_imports.rs`:33 on 2023-02-10 22:30_

I think this should be a "sometimes" autofixable. You can grep for `Availability::Sometimes` to see examples of that pattern.

---

_Label `autofix` added by @charliermarsh on 2023-02-11 03:05_

---

_Merged by @charliermarsh on 2023-02-11 03:05_

---

_Closed by @charliermarsh on 2023-02-11 03:05_

---

_Comment by @gertvdijk on 2023-02-11 14:59_

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

---

_Branch deleted on 2023-02-11 15:01_

---
