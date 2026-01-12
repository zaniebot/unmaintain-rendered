```yaml
number: 3819
title: "uv.toml doesn't seem to respect/support this:"
type: issue
state: closed
author: charliermarsh
labels:
  - configuration
assignees: []
created_at: 2024-05-24T12:41:16Z
updated_at: 2024-05-24T13:31:47Z
url: https://github.com/astral-sh/uv/issues/3819
synced_at: 2026-01-12T15:58:46Z
```

# uv.toml doesn't seem to respect/support this:

---

_@charliermarsh_

              uv.toml doesn't seem to respect/support this:

```
venv/bin/python -m uv pip install --verbose --upgrade pip-tools
error: Failed to parse `uv.toml`
  Caused by: TOML parse error at line 2, column 18
  |
2 | index-strategy = "unsafe-any-match"
  |                  ^^^^^^^^^^^^^^^^^^
unknown variant `unsafe-any-match`, expected one of `first-index`, `unsafe-first-match`, `unsafe-best-match`
```

_Originally posted by @korhojoa in https://github.com/astral-sh/uv/issues/2775#issuecomment-2128845629_
            

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-24 12:41_

---

_Label `configuration` added by @charliermarsh on 2024-05-24 12:41_

---

_Closed by @charliermarsh on 2024-05-24 13:31_

---
