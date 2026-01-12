```yaml
number: 2576
title: "`B007` causes infinite loop for variable suffixed with `_`"
type: issue
state: closed
author: spaceone
labels:
  - bug
assignees: []
created_at: 2023-02-05T10:20:51Z
updated_at: 2023-02-05T13:01:09Z
url: https://github.com/astral-sh/ruff/issues/2576
synced_at: 2026-01-12T15:54:43Z
```

# `B007` causes infinite loop for variable suffixed with `_`

---

_@spaceone_

```
$ ruff --fix --diff --isolated --select B007 foo.py

error: Failed to converge after 100 iterations.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `foo.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

--- foo.py
+++ foo.py
@@ -1,2 +1,2 @@
-for line_ in range(self.header_lines):
+for ____________________________________________________________________________________________________line_ in range(self.header_lines):
     fp.readline()

Would fix 100 errors.
```

---

_Renamed from "`B0` infinite loop" to "`B070` infinite loop" by @spaceone on 2023-02-05 10:22_

---

_Renamed from "`B070` infinite loop" to "`B007` infinite loop" by @spaceone on 2023-02-05 10:22_

---

_Renamed from "`B007` infinite loop" to "`B007` causes infinite loop for variable suffixed with `_`" by @spaceone on 2023-02-05 10:22_

---

_Comment by @spaceone on 2023-02-05 10:23_

in such a case the original file should not be rewritten!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-05 12:50_

---

_Label `bug` added by @charliermarsh on 2023-02-05 12:50_

---

_Closed by @charliermarsh on 2023-02-05 13:01_

---
