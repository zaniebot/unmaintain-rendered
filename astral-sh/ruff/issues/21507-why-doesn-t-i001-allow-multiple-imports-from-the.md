---
number: 21507
title: "Why doesn't I001 allow multiple imports from the same package on one line?"
type: issue
state: closed
author: llamafilm
labels:
  - question
assignees: []
created_at: 2025-11-18T07:07:24Z
updated_at: 2025-11-21T06:56:29Z
url: https://github.com/astral-sh/ruff/issues/21507
synced_at: 2026-01-10T01:23:02Z
---

# Why doesn't I001 allow multiple imports from the same package on one line?

---

_Issue opened by @llamafilm on 2025-11-18 07:07_

### Question

Ruff rule I001 is flagging when I have multiple imports from the same package on one line.  Why does it do this?  If the goal is to follow PEP 8, it gives this example:

```py
# Correct:
import os
import sys

# Wrong:
import sys, os

# Correct:
from subprocess import Popen, PIPE
```

Ruff says that last line is wrong.

### Version

ruff 0.14.4

---

_Label `question` added by @llamafilm on 2025-11-18 07:07_

---

_Comment by @amyreese on 2025-11-18 17:13_

Ruff uses case-sensitive sorting for imported names, so it wants to reorder those names:

```
$ uvx ruff check --select I001 --diff foo.py
--- foo.py
+++ foo.py
@@ -1 +1 @@
-from subprocess import Popen, PIPE
+from subprocess import PIPE, Popen

Would fix 1 error.
> [1]
``` 

---

_Comment by @llamafilm on 2025-11-19 03:51_

That doesn't work for me.

```
% uvx ruff check --select I001 --diff 21507.py
--- 21507.py
+++ 21507.py
@@ -1,2 +1,3 @@
-from subprocess import PIPE, Popen
+from subprocess import PIPE
+from subprocess import Popen
 

Would fix 1 error.
```

---

_Comment by @MichaReiser on 2025-11-19 07:18_

Can you run `uvx ruff check --show-settings 21507.py` and share the output with us?

---

_Comment by @llamafilm on 2025-11-21 04:32_

Ok I found it!  Thanks for pointing me in the right direction.  It was caused by this in pyproject.toml, a default choice from django-cookiecutter.

```toml
[tool.ruff.lint.isort]
force-single-line = true
```

---

_Closed by @MichaReiser on 2025-11-21 06:56_

---
