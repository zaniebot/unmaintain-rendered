```yaml
number: 552
title: Remove extend-select and extend-ignore from pyproject.toml
type: issue
state: closed
author: charliermarsh
labels:
  - breaking
assignees: []
created_at: 2022-11-02T23:02:54Z
updated_at: 2022-12-16T05:26:50Z
url: https://github.com/astral-sh/ruff/issues/552
synced_at: 2026-01-10T12:05:21Z
```

# Remove extend-select and extend-ignore from pyproject.toml

---

_Issue opened by @charliermarsh on 2022-11-02 23:02_

Flake8 has a lot of implicit behavior, and I'd like to make Ruff more explicit. For example, I think users should really be specifying a `select` rather than relying on auto-enabled plugins. If users are specifying a `select`, there's no need for `extend-select`, except as a command-line convenience.


---

_Label `breaking` added by @charliermarsh on 2022-11-03 02:13_

---

_Comment by @andersk on 2022-11-03 21:51_

Seems reasonable now that the default is just `["E", "F"]`.

---

_Comment by @andersk on 2022-11-13 01:56_

These `extend-` directives might actually be useful if we end up supporting something like `overrides` à la [ESLint](https://eslint.org/docs/latest/user-guide/configuring/configuration-files#how-do-overrides-work) and [mypy](https://mypy.readthedocs.io/en/stable/config_file.html#using-a-pyproject-toml-file):

```toml
[tool.ruff]
select = ["B", "E", "F", "S", "T"]
ignore = ["B008", "E402", "E5"]

[[tool.ruff.overrides]]
files = ["api/**"]
extend-select = ["D1"]

[[tool.ruff.overrides]]
files = ["**/tests/**", "**/test_*.py"]
extend-ignore = ["T", "S101"]
```

---

_Comment by @charliermarsh on 2022-12-16 05:26_

These now have new, better semantics.

---

_Closed by @charliermarsh on 2022-12-16 05:26_

---
