```yaml
number: 928
title: Use better style for per-file-ignores
type: issue
state: closed
author: lovetox
labels:
  - documentation
assignees: []
created_at: 2022-11-27T22:26:01Z
updated_at: 2022-11-28T02:38:44Z
url: https://github.com/astral-sh/ruff/issues/928
synced_at: 2026-01-12T15:54:40Z
```

# Use better style for per-file-ignores

---

_@lovetox_

currently the docs say to use an inline table

`per-file-ignores = {"__init__.py" = ["E402"], "path/to/file.py" = ["E402"]}`

inline tables allow no newlines, and are intended for really small tables.
In bigger codebases this will not stay small, and an inline table would force you to describe everything on one line.

I tested it and there is another representation that just works fine

i wasted 30 minutes to find this, so i thought maybe you are interested in it

```
[tool.ruff.per-file-ignores]
"__init__.py" = ["E402"]
"path/to/file.py" = ["E402"]
```

---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-27 22:27_

---

_Label `documentation` added by @charliermarsh on 2022-11-27 22:27_

---

_Comment by @charliermarsh on 2022-11-27 22:28_

Yeah those are equivalent representations, it's more of a TOML thing than any specific choice made in Ruff. I'm sorry you lost time on it! I'll change the docs to use the non-inline syntax, for clarity.


---

_Closed by @charliermarsh on 2022-11-28 02:38_

---
