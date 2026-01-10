---
number: 11138
title: "`ruff server` does not respect `exclude` configuration"
type: issue
state: closed
author: snowsignal
labels:
  - configuration
  - server
assignees: []
created_at: 2024-04-25T02:02:23Z
updated_at: 2024-05-22T03:14:19Z
url: https://github.com/astral-sh/ruff/issues/11138
synced_at: 2026-01-10T01:22:50Z
---

# `ruff server` does not respect `exclude` configuration

---

_Issue opened by @snowsignal on 2024-04-25 02:02_

At the moment, `ruff server` does not yet respect `exclude` configuration, either in the editor configuration or in workspace configuration. This may cause unexpected behavior when editing an excluded file.

---

_Label `server` added by @snowsignal on 2024-04-25 02:02_

---

_Assigned to @snowsignal by @snowsignal on 2024-04-25 02:02_

---

_Label `configuration` added by @snowsignal on 2024-04-25 02:02_

---

_Comment by @hoxbro on 2024-04-28 13:03_

I also see a problem with `[tool.ruff.lint.per-file-ignores]` not being respected. 

---

_Referenced in [astral-sh/ruff#11185](../../astral-sh/ruff/issues/11185.md) on 2024-04-28 13:07_

---

_Comment by @charliermarsh on 2024-04-28 13:07_

Yeah that's not wired up yet, thanks -- filing separately here: https://github.com/astral-sh/ruff/issues/11185

---

_Comment by @charliermarsh on 2024-05-22 03:14_

I think this should now be working, since we set the filename.

---

_Closed by @charliermarsh on 2024-05-22 03:14_

---
