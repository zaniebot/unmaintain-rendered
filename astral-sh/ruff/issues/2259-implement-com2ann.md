```yaml
number: 2259
title: Implement com2ann?
type: issue
state: closed
author: spaceone
labels:
  - plugin
assignees: []
created_at: 2023-01-27T14:54:16Z
updated_at: 2023-02-17T21:00:42Z
url: https://github.com/astral-sh/ruff/issues/2259
synced_at: 2026-01-10T11:09:45Z
```

# Implement com2ann?

---

_Issue opened by @spaceone on 2023-01-27 14:54_

https://pypi.org/project/com2ann/ is a nice project, which could be considered to be added.
it tansforms type comments into real annotations:

```diff
-def foo():
-    # type: () -> None
+def foo() -> None:
```

---

_Label `plugin` added by @charliermarsh on 2023-01-27 15:31_

---

_Comment by @charliermarsh on 2023-02-17 21:00_

I think I want to cut scope here, because I've generally tried to avoid implementing rules that feel like one-time 2-to-3 (or 3.5-to-3.7 etc.) upgrades (as opposed to upgrades that are consistently useful on Python 3 code).

---

_Closed by @charliermarsh on 2023-02-17 21:00_

---
