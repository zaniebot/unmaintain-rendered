```yaml
number: 13390
title: PLE1205 false-positive with Loguru
type: issue
state: open
author: dorschw
labels:
  - rule
  - type-inference
assignees: []
created_at: 2024-09-18T09:33:35Z
updated_at: 2024-09-19T14:11:38Z
url: https://github.com/astral-sh/ruff/issues/13390
synced_at: 2026-01-10T11:09:55Z
```

# PLE1205 false-positive with Loguru

---

_Issue opened by @dorschw on 2024-09-18 09:33_

In [loguru](https://github.com/Delgan/loguru), the formatting isn't `%`-based, but `{}`-based, see [here](https://loguru.readthedocs.io/en/stable/resources/migration.html).
When configuring it as the logger object and writing
```python
logger.info("{}", var)
```
a false-positive PLE1205 is raised. 

---

_Comment by @MackHalliday on 2024-09-18 22:15_

I am having this same issue - It's so annoying. Please lmk if you find a workaround. 

---

_Label `type-inference` added by @MichaReiser on 2024-09-19 07:07_

---

_Comment by @MichaReiser on 2024-09-19 07:10_

The PLE1205 for detecting logger's is very basic at the moment. All it does is match on the variable name (this does not apply for `logging` calls). This is because Ruff has no type inference capabilities (we're working on it) that allow reasoning about how values flow through the program. This means there's no immediate fix here. 

I recommend you disable `PLE1205` in your project if you're consistently using (`ignore = ["PLE1205"]`) or ignore it on a per file level: `per-file-ignores = { "file.py": ["PLE1205"]}`

---

_Label `rule` added by @MichaReiser on 2024-09-19 07:11_

---

_Comment by @MackHalliday on 2024-09-19 14:11_

Thank you for the response :D 

---
