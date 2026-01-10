---
number: 3318
title: "Autofix Error (TID252): autofixing relative imports on namespaced packages causes invalid code and ruff doesn't always know it is invalid"
type: issue
state: closed
author: onerandomusername
labels:
  - bug
  - question
assignees: []
created_at: 2023-03-03T01:59:02Z
updated_at: 2023-03-03T05:06:12Z
url: https://github.com/astral-sh/ruff/issues/3318
synced_at: 2026-01-10T01:22:41Z
---

# Autofix Error (TID252): autofixing relative imports on namespaced packages causes invalid code and ruff doesn't always know it is invalid

---

_Issue opened by @onerandomusername on 2023-03-03 01:59_

### Summary

note: a recreation can be found here: https://github.com/onerandomusername/bugs-ruff-import-autofix/tree/ruff-namespaced-tid252, and a large codebase that uses relative imports can be found here https://github.com/DisnakeDev/disnake/tree/6a1ea9de03671efa60542bdc576e30c10f746a20

The offending file above is [`solar_system/namespace/rover/__init__.py`](https://github.com/onerandomusername/bugs-ruff-import-autofix/blob/b1d513316873109c872baeebf878b576849e9d20/solar_system/namespace/rover/__init__.py), which has the following code.

**command: `ruff . --select TID252 --fix`**
```py
from ... import PLANET

from ...mars import POSITION_FROM_SUN
```

result (after removing the first line):
```py
from mars import POSITION_FROM_SUN 
```

Because it is in a namespaced module, ruff fails to understand the namespace and provides the wrong module to the absolute imports--if it provides them at all!

ruff version: 0.0.253

Output of the invalid code:
```sh
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `solar_system/namespace/rover/__init__.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!
```

---

_Referenced in [DisnakeDev/disnake#951](../../DisnakeDev/disnake/pulls/951.md) on 2023-03-03 03:40_

---

_Comment by @charliermarsh on 2023-03-03 03:51_

If I add:

```toml
[tool.ruff]
namespace-packages = ["solar_system/namespace"]
```

to `./pyproject.toml` in the example repo, I get:

```diff
diff --git a/solar_system/namespace/rover/__init__.py b/solar_system/namespace/rover/__init__.py
index d885330..1c06ef9 100644
--- a/solar_system/namespace/rover/__init__.py
+++ b/solar_system/namespace/rover/__init__.py
@@ -1,3 +1,3 @@
-from ... import PLANET # this causes invalid code, and ruff realises it.
+from solar_system import PLANET # this causes invalid code, and ruff realises it.

-from ...mars import POSITION_FROM_SUN # this is formatted to the wrong module, but ruff doesn't catch it
+from solar_system.mars import POSITION_FROM_SUN # this is formatted to the wrong module, but ruff doesn't catch it
```

Which seems right, but can you confirm?

---

_Label `question` added by @charliermarsh on 2023-03-03 03:52_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-03 03:59_

---

_Label `bug` added by @charliermarsh on 2023-03-03 03:59_

---

_Referenced in [astral-sh/ruff#3319](../../astral-sh/ruff/pulls/3319.md) on 2023-03-03 03:59_

---

_Closed by @charliermarsh on 2023-03-03 05:03_

---

_Comment by @charliermarsh on 2023-03-03 05:04_

(The syntax error is fixed, so closing, but LMK if you don't see the actual correct behavior after explicitly marking the namespace package.)

---

_Comment by @onerandomusername on 2023-03-03 05:06_

With that setting enabled *both* worked properly, still using 0.0.253. Unfortunately, I tried that setting on disnake and found a separate but related bug. Will open another issue for that one.

---
