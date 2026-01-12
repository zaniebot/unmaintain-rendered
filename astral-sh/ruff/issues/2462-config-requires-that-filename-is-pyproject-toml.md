```yaml
number: 2462
title: "`--config` requires that filename is `pyproject.toml` or `ruff.toml`"
type: issue
state: closed
author: charliermarsh
labels:
  - cli
assignees: []
created_at: 2023-02-02T01:48:29Z
updated_at: 2023-02-02T02:35:44Z
url: https://github.com/astral-sh/ruff/issues/2462
synced_at: 2026-01-12T15:54:42Z
```

# `--config` requires that filename is `pyproject.toml` or `ruff.toml`

---

_@charliermarsh_

We need a way to infer the schema... I think we could loosen this, and assume that it's a `ruff.toml` if it's not named `pyproject.toml`? Since `pyproject.toml` is a special name, but `ruff.toml` is just something we made up.


---

_Label `cli` added by @charliermarsh on 2023-02-02 01:48_

---

_Closed by @charliermarsh on 2023-02-02 02:35_

---
