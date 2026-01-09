---
number: 835
title: Add the possibility to add import and respect the import sorting.
type: issue
state: closed
author: JonathanPlasse
labels:
  - core
assignees: []
created_at: 2022-11-20T20:11:03Z
updated_at: 2023-03-30T15:33:48Z
url: https://github.com/astral-sh/ruff/issues/835
synced_at: 2026-01-07T13:12:14-06:00
---

# Add the possibility to add import and respect the import sorting.

---

_Issue opened by @JonathanPlasse on 2022-11-20 20:11_

- In #816, we need to add the `sys` module if missing and keep the imports sorted.

---

_Label `enhancement` added by @charliermarsh on 2022-11-20 21:51_

---

_Comment by @JonathanPlasse on 2022-11-22 22:24_

@charliermarsh, does #875 facilitate the implementation of this issue?

---

_Referenced in [astral-sh/ruff#1038](../../astral-sh/ruff/issues/1038.md) on 2022-12-04 17:02_

---

_Referenced in [astral-sh/ruff#1422](../../astral-sh/ruff/issues/1422.md) on 2022-12-28 14:15_

---

_Referenced in [astral-sh/ruff#1507](../../astral-sh/ruff/issues/1507.md) on 2022-12-31 09:22_

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:18_

---

_Label `internal` added by @charliermarsh on 2022-12-31 18:18_

---

_Label `internal` removed by @charliermarsh on 2023-01-05 04:58_

---

_Label `good first issue` added by @charliermarsh on 2023-01-05 04:58_

---

_Label `core` added by @charliermarsh on 2023-01-05 04:58_

---

_Comment by @charliermarsh on 2023-01-05 05:01_

This would be good to tackle. It has now come up a few times now.

I think the idea would be to track the imports that need to be present for a given code modification, and then do a pass at the end to "add" those imports. It would somehow have to be connected to the autofix API too, to ensure that we only add the import if we end up applying the fix.

You can see how LibCST handles the problem [here](https://libcst.readthedocs.io/en/latest/_modules/libcst/codemod/visitors/_add_imports.html).


---

_Comment by @charliermarsh on 2023-01-05 05:02_

There are a few sub-problems:

- How do we determine where the imports should go? It's probably not necessary to put the imports in the "right" spot, since we can rely on the next autofix iteration fixing up the order. But they should go in the first "block".
- How do we only add imports we don't already have?
- ...

---

_Referenced in [astral-sh/ruff#1640](../../astral-sh/ruff/issues/1640.md) on 2023-01-05 05:04_

---

_Referenced in [astral-sh/ruff#1913](../../astral-sh/ruff/issues/1913.md) on 2023-01-17 12:39_

---

_Referenced in [astral-sh/ruff#1522](../../astral-sh/ruff/issues/1522.md) on 2023-01-31 12:20_

---

_Referenced in [astral-sh/ruff#2348](../../astral-sh/ruff/pulls/2348.md) on 2023-01-31 13:00_

---

_Referenced in [astral-sh/ruff#2422](../../astral-sh/ruff/issues/2422.md) on 2023-01-31 22:00_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-02 03:14_

---

_Label `good first issue` removed by @charliermarsh on 2023-02-17 23:45_

---

_Referenced in [astral-sh/ruff#3284](../../astral-sh/ruff/issues/3284.md) on 2023-03-01 23:28_

---

_Referenced in [astral-sh/ruff#3510](../../astral-sh/ruff/issues/3510.md) on 2023-03-14 14:53_

---

_Comment by @charliermarsh on 2023-03-22 23:36_

I am returning to this.

---

_Comment by @charliermarsh on 2023-03-28 17:58_

Getting close...

```diff
diff --git a/foo.py b/foo.py
index 0647ee33b..696a9e873 100644
--- a/foo.py
+++ b/foo.py
@@ -1,3 +1,4 @@
+import sys
 exit = 1

-quit(1)
+sys.exit(1)
```

---

_Referenced in [astral-sh/ruff#3787](../../astral-sh/ruff/pulls/3787.md) on 2023-03-28 22:17_

---

_Closed by @charliermarsh on 2023-03-30 15:33_

---
