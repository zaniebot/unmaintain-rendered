```yaml
number: 8757
title: "`uvx` complains about non-existent `--with`"
type: issue
state: open
author: charliermarsh
labels:
  - help wanted
  - error messages
assignees: []
created_at: 2024-11-01T17:25:36Z
updated_at: 2024-11-02T14:11:14Z
url: https://github.com/astral-sh/uv/issues/8757
synced_at: 2026-01-12T15:59:34Z
```

# `uvx` complains about non-existent `--with`

---

_@charliermarsh_

Reported on Discord:
```
❯ uvx --from ./python/ruff-ecosystem ruff-ecosystem --output-format markdown --force-preview --cache ../ecosystem format ./target/release/ruff ./target/debug/ruff > changes.md
  × Invalid `--with` requirement
  ╰─▶ Distribution not found at: file:///home/astral/ruff/python/python/ruff-ecosystem
```

---

_Label `help wanted` added by @charliermarsh on 2024-11-01 17:25_

---

_Label `error messages` added by @charliermarsh on 2024-11-01 17:25_

---

_Comment by @Aryakoste on 2024-11-02 04:34_

Hii @charliermarsh I would like to work for PR for this issue. 

---

_Comment by @zanieb on 2024-11-02 14:11_

@Aryakoste go for it

---
