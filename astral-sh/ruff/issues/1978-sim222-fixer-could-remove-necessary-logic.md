---
number: 1978
title: SIM222 fixer could remove necessary logic
type: issue
state: closed
author: spaceone
labels: []
assignees: []
created_at: 2023-01-18T23:25:35Z
updated_at: 2023-01-19T00:08:15Z
url: https://github.com/astral-sh/ruff/issues/1978
synced_at: 2026-01-10T01:22:40Z
---

# SIM222 fixer could remove necessary logic

---

_Issue opened by @spaceone on 2023-01-18 23:25_

I have a validation function which relies on a raised exception by `json.loads()` but that one is removed:

```
     def validate(self, value):
         # type: (str) -> object
-        return json.loads(value) or True
+        return True
```



---

_Referenced in [astral-sh/ruff#1984](../../astral-sh/ruff/pulls/1984.md) on 2023-01-18 23:59_

---

_Closed by @charliermarsh on 2023-01-19 00:08_

---

_Referenced in [astral-sh/ruff#2397](../../astral-sh/ruff/issues/2397.md) on 2023-01-31 14:22_

---
