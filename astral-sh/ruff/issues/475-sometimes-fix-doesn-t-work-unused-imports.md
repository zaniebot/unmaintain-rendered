```yaml
number: 475
title: "Sometimes --fix doesn't work (unused imports)"
type: issue
state: closed
author: fsouza
labels: []
assignees: []
created_at: 2022-10-26T16:49:53Z
updated_at: 2022-10-26T20:36:13Z
url: https://github.com/astral-sh/ruff/issues/475
synced_at: 2026-01-12T15:54:40Z
```

# Sometimes --fix doesn't work (unused imports)

---

_@fsouza_

I haven't been able to dig into this yet, but sometimes `--fix` doesn't remove unused imports (but it does claim to have fixed the issue). Reproducing with the autoflake repo:

```
$ git clone https://github.com/pycqa/autoflake.git /tmp/autoflake
$ cd /tmp/autoflake
$ patch -p1 <<EOF
\ diff --git a/autoflake.py b/autoflake.py
index 2b7b917..12a9f4f 100755
--- a/autoflake.py
+++ b/autoflake.py
@@ -38,6 +38,7 @@ import tokenize
 import pyflakes.api
 import pyflakes.messages
 import pyflakes.reporter
+from pyflakes.checker import Annotation


 __version__ = "1.7.7"
EOF
$ ruff autoflake.py
Found 1 error(s).
autoflake.py:41:1: F401 `pyflakes.checker.Annotation` imported but unused
1 potentially fixable with the --fix option.
％ ruff --fix autoflake.py
Found 0 error(s) (1 fixed).
％ ruff autoflake.py
Found 1 error(s).
autoflake.py:41:1: F401 `pyflakes.checker.Annotation` imported but unused
1 potentially fixable with the --fix option.
```

Note how it claims to have fixed the error ("Found 0 error(s) (1 fixed)."), but it didn't.

I can dig into it before the end of the week, but reporting just in case someone beats me to it :)

---

_Comment by @charliermarsh on 2022-10-26 16:54_

Awesome, thanks for reporting!

---

_Assigned to @charliermarsh by @charliermarsh on 2022-10-26 20:05_

---

_Comment by @charliermarsh on 2022-10-26 20:09_

Reproduced, I see the issue.

---

_Closed by @charliermarsh on 2022-10-26 20:36_

---
