---
number: 1439
title: Subsequent runs with --diff give no result
type: issue
state: closed
author: rhkleijn
labels:
  - bug
assignees: []
created_at: 2022-12-29T16:05:25Z
updated_at: 2022-12-29T18:32:08Z
url: https://github.com/astral-sh/ruff/issues/1439
synced_at: 2026-01-10T01:22:39Z
---

# Subsequent runs with --diff give no result

---

_Issue opened by @rhkleijn on 2022-12-29 16:05_

I tried the new `--diff` functionality from #1430 / #1431 and found that it works nicely on the first run, but subsequent runs give no result.
To reproduce:

```bash
$ echo "import os" > main.py
$ ruff main.py --diff
```
gives this output
```
--- main.py
+++ main.py
@@ -1 +0,0 @@
-import os

Would fix 1 error(s).
```
as expected.

But this second call produces no output:
```bash
$ ruff main.py --diff
```

When I manually remove `.ruff-cache` it works again on the next run. I am not familiar with ruff's internals but it might be some interaction with caching.



---

_Comment by @charliermarsh on 2022-12-29 16:45_

Ah yeah, this makes sense. Will fix.

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-29 16:45_

---

_Label `bug` added by @charliermarsh on 2022-12-29 16:45_

---

_Comment by @charliermarsh on 2022-12-29 16:46_

For now you can run with -n to disable the cache. (It shouldn’t be noticeably slower unless you have an enormous codebase.)

---

_Referenced in [astral-sh/ruff#1441](../../astral-sh/ruff/pulls/1441.md) on 2022-12-29 17:51_

---

_Closed by @charliermarsh on 2022-12-29 17:51_

---

_Comment by @charliermarsh on 2022-12-29 18:32_

Going out in [v0.0.200](https://github.com/charliermarsh/ruff/releases/tag/v0.0.200).

---
