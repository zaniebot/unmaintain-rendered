```yaml
number: 2053
title: Support for --builtins-ignorelist in flake8-builtins checks
type: issue
state: closed
author: ehdr
labels:
  - good first issue
assignees: []
created_at: 2023-01-21T09:02:08Z
updated_at: 2023-01-21T18:04:32Z
url: https://github.com/astral-sh/ruff/issues/2053
synced_at: 2026-01-12T15:54:42Z
```

# Support for --builtins-ignorelist in flake8-builtins checks

---

_@ehdr_

[flake8-builtins](https://pypi.org/project/flake8-builtins/) provides a [`--builtins-ignorelist` option](https://pypi.org/project/flake8-builtins/#options) that lets you ignore a custom list of builtins, e.g.:
```
flake8 --builtins-ignorelist id,copyright *.py
```

Have you considered adding the corresponding config option to ruff? I'd imagine something like
```
[tool.ruff.flake8-builtins]
builtins-ignorelist = ["id", "copyright"]
```


---

_Label `good first issue` added by @charliermarsh on 2023-01-21 12:15_

---

_Comment by @charliermarsh on 2023-01-21 12:15_

Yeah we can definitely support that.

---

_Closed by @charliermarsh on 2023-01-21 16:09_

---

_Comment by @ehdr on 2023-01-21 17:55_

Thanks @charliermarsh and @saadmk11 !

---

_Comment by @charliermarsh on 2023-01-21 18:04_

All @saadmk11 :)

---
