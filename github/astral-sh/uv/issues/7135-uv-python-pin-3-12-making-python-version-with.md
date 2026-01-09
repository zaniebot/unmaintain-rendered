---
number: 7135
title: "`uv python pin 3.12` making `.python-version` with trailing whitespace"
type: issue
state: closed
author: jamesbraza
labels:
  - bug
  - good first issue
  - help wanted
assignees: []
created_at: 2024-09-06T19:53:37Z
updated_at: 2024-09-06T22:03:53Z
url: https://github.com/astral-sh/uv/issues/7135
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv python pin 3.12` making `.python-version` with trailing whitespace

---

_Issue opened by @jamesbraza on 2024-09-06 19:53_

With `uv==0.4.6`, running `uv python pin 3.12`, the made `.python-version` file does not have trailing whitespace.

This is a little suboptimal because:

- GitHub will show the "missing trailing whitespace" red icon in the diff
- If using https://github.com/pre-commit/pre-commit-hooks's `trailing-whitespace`, it will give a nonzero return code and autofix the `.python-version` file to have trailing whitespace

Can we have `uv python pin` either:
- By default create the `.python-version` file with trailing whitespace
- Add a configuration/CLI flag `--trailing-whitespace` for `uv python pin` to add the trailing whitespace

---

_Comment by @charliermarsh on 2024-09-06 19:54_

Ah sorry. It doesn't have a trailing newline? That's my bad.

---

_Label `bug` added by @charliermarsh on 2024-09-06 19:54_

---

_Label `good first issue` added by @charliermarsh on 2024-09-06 19:54_

---

_Label `help wanted` added by @charliermarsh on 2024-09-06 19:54_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-06 21:47_

---

_Referenced in [astral-sh/uv#7140](../../astral-sh/uv/pulls/7140.md) on 2024-09-06 21:55_

---

_Closed by @charliermarsh on 2024-09-06 22:03_

---
