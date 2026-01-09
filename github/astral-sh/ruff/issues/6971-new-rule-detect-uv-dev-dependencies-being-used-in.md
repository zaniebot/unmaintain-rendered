---
number: 6971
title: new rule - detect uv dev dependencies being used in production code
type: issue
state: open
author: DetachHead
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-08-29T10:36:46Z
updated_at: 2024-10-02T05:32:55Z
url: https://github.com/astral-sh/ruff/issues/6971
synced_at: 2026-01-07T13:12:15-06:00
---

# new rule - detect uv dev dependencies being used in production code

---

_Issue opened by @DetachHead on 2023-08-29 10:36_

in my project, i was importing from dev dependencies in intended to be used by users of my module:

```py
# public_module.py
from foo import bar
```
```py
# pyproject.toml
[tool.poetry.group.dev.dependencies]
foo = "^1.0.0"
```

because of this, all my tests were passing because the dependency existed, but the moment a user tried to use it, it crashed because the module was not installed.

it would be nice if ruff had a rule to detect this like [this eslint plugin does](https://github.com/import-js/eslint-plugin-import/blob/v2.27.5/docs/rules/no-extraneous-dependencies.md)

---

_Comment by @KotlinIsland on 2023-08-29 12:27_

Not just dev, but any non-main group

---

_Label `rule` added by @zanieb on 2023-08-29 13:53_

---

_Label `needs-decision` added by @charliermarsh on 2023-08-30 00:43_

---

_Renamed from "new rule - detect poetry dev dependencies being used in production code" to "new rule - detect poetry/pdm dev dependencies being used in production code" by @DetachHead on 2023-10-01 08:22_

---

_Comment by @DetachHead on 2023-10-01 08:24_

also pdm dev dependencies:
```toml
[tool.pdm.dev-dependencies]
lint = ["foo>=1.0.0"]
```

---

_Comment by @indigoviolet on 2023-10-17 18:41_

Same issue exists with rye:

```toml
[tool.rye]
managed = true
dev-dependencies = [
    "ipykernel>=6.24.0",
...
```

---

_Comment by @zanieb on 2023-10-17 20:36_

I foresee the major issue here being that dependency names do not map one-to-one with imported module names. How can we know which maps to which statically?

---

_Comment by @charliermarsh on 2023-10-17 20:47_

Unfortunately not possible to know that in advance (unless we hard-code a database of lookups) -- you need access to the built distribution or the virtual environment.

---

_Comment by @KotlinIsland on 2024-09-30 03:54_

@charliermarsh now that astral controls the entire stack, couldn't `uv` be used to resolve this?

---

_Renamed from "new rule - detect poetry/pdm dev dependencies being used in production code" to "new rule - detect uv dev dependencies being used in production code" by @DetachHead on 2024-09-30 04:14_

---

_Comment by @UnknownPlatypus on 2024-09-30 21:35_

I believe https://github.com/fpgmaas/deptry does this and supports Uv, poetry, pip and pdm. (And btw, it's relying on ruff parser for the import detections)

---

_Comment by @DetachHead on 2024-10-02 05:32_

thanks, though it would be nice if this functionality was built into ruff, mainly for its language server so that the errors it reports can be visible in your IDE

---
