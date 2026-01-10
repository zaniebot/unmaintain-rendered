```yaml
number: 1982
title: PIE790 introduces E303
type: issue
state: closed
author: spaceone
labels:
  - wontfix
assignees: []
created_at: 2023-01-18T23:47:16Z
updated_at: 2023-01-19T17:12:22Z
url: https://github.com/astral-sh/ruff/issues/1982
synced_at: 2026-01-10T11:09:44Z
```

# PIE790 introduces E303

---

_Issue opened by @spaceone on 2023-01-18 23:47_

```
$ ruff --select PIE790 --fix --diff foo.py 
--- foo.py
+++ foo.py
@@ -1,7 +1,6 @@
 class Foo:
     """foo"""
 
-    pass
 
 
 class Bar:
```

```
$ pycodestyle foo.py
foo.py:6:1: E303 too many blank lines (3)
```

---

_Label `wontfix` added by @charliermarsh on 2023-01-19 15:30_

---

_Comment by @charliermarsh on 2023-01-19 15:31_

To be honest, while I appreciate the issue, I think we're unlikely to fix these kinds of errors in a one-off fashion since we don't support those stylistic rules right now anyway. These could either be fixed by eventually supporting this roles, or with the completion of the autoformatter work (#1904).

---

_Closed by @charliermarsh on 2023-01-19 15:31_

---

_Comment by @spaceone on 2023-01-19 17:12_

OK. I basically don't need an auto-formatter but the basic rules from https://github.com/charliermarsh/ruff/issues/1073. If they are available I can replace `flake8` (and if the fixers are available even `autopep8` can be replaced).

---
