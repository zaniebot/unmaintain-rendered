```yaml
number: 5856
title: "Warn when project options are used with `uv run` outside a project"
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - tracing
  - cli
  - preview
assignees: []
created_at: 2024-08-07T13:11:56Z
updated_at: 2024-08-09T23:10:35Z
url: https://github.com/astral-sh/uv/issues/5856
synced_at: 2026-01-12T15:58:59Z
```

# Warn when project options are used with `uv run` outside a project

---

_@zanieb_

e.g., `--dev`, `--no-dev`, `--extra`, `--all-extras`, `--no-all-extras`

Maybe , `--no-project`.

These appear to be ignored right now, e.g.

```
❯ uv run --dev --extra foo script.py
warning: `uv run` is experimental and may change without warning
Reading inline script metadata from: script.py
```

---

_Label `tracing` added by @zanieb on 2024-08-07 13:12_

---

_Label `cli` added by @zanieb on 2024-08-07 13:12_

---

_Label `help wanted` added by @charliermarsh on 2024-08-09 02:53_

---

_Label `preview` added by @charliermarsh on 2024-08-09 02:53_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-09 14:38_

---

_Closed by @charliermarsh on 2024-08-09 23:10_

---
