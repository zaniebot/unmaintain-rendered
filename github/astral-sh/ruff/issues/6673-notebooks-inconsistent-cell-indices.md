---
number: 6673
title: "Notebooks: Inconsistent cell indices"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
assignees: []
created_at: 2023-08-18T08:26:14Z
updated_at: 2023-09-26T14:14:01Z
url: https://github.com/astral-sh/ruff/issues/6673
synced_at: 2026-01-07T13:12:15-06:00
---

# Notebooks: Inconsistent cell indices

---

_Issue opened by @MichaReiser on 2023-08-18 08:26_

Diagnostics use one based cell indices:

```
/home/micha/astral/test/test-notebook.ipynb:cell 1:2:8: F401 [*] `math` imported but unused
/home/micha/astral/test/test-notebook.ipynb:cell 5:1:8: F811 Redefinition of unused `random` from line 1
/home/micha/astral/test/test-notebook.ipynb:cell 5:2:8: F401 [*] `pprint` imported but unused
/home/micha/astral/test/test-notebook.ipynb:cell 7:2:4: F632 [*] Use `==` to compare constant literals
/home/micha/astral/test/test-notebook.ipynb:cell 7:3:38: F632 [*] Use `==` to compare constant literals
Found 5 errors.
[*] 4 potentially fixable with the --fix option.
```

Diffs use zero based cell indices

```
--- /home/micha/astral/test/test-notebook.ipynb:cell 0
+++ /home/micha/astral/test/test-notebook.ipynb:cell 0
@@ -1,2 +1 @@
-import random
-import math
+import random
--- /home/micha/astral/test/test-notebook.ipynb:cell 4
+++ /home/micha/astral/test/test-notebook.ipynb:cell 4
@@ -1,4 +1,3 @@
 import random
-import pprint
 
 random.randint(10, 20)
--- /home/micha/astral/test/test-notebook.ipynb:cell 6
+++ /home/micha/astral/test/test-notebook.ipynb:cell 6
@@ -1,3 +1,3 @@
 foo = 1
-if foo is 2:
-    raise ValueError(f"Invalid foo: {foo is 1}")
+if foo == 2:
+    raise ValueError(f"Invalid foo: {foo == 1}")

Would fix 4 errors.
```

## Expected

The indices to be consistent between diagnostics and the diff mode. I'm leaning towards one based indices because that's what we use for row/column too.


---

_Label `bug` added by @MichaReiser on 2023-08-18 08:27_

---

_Referenced in [astral-sh/ruff#7662](../../astral-sh/ruff/pulls/7662.md) on 2023-09-26 06:25_

---

_Closed by @dhruvmanila on 2023-09-26 14:14_

---
