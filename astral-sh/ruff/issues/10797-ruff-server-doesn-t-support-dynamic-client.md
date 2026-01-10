```yaml
number: 10797
title: "`ruff server` doesn't support dynamic client settings in workspaces"
type: issue
state: closed
author: snowsignal
labels:
  - server
assignees: []
created_at: 2024-04-05T23:26:17Z
updated_at: 2024-06-17T16:31:28Z
url: https://github.com/astral-sh/ruff/issues/10797
synced_at: 2026-01-10T11:09:53Z
```

# `ruff server` doesn't support dynamic client settings in workspaces

---

_Issue opened by @snowsignal on 2024-04-05 23:26_

This is a tracking issue for supporting dynamic client settings in workspaces.

Right now, client settings are created during server initialization and remain static while the server runs. This causes issues when workspace folders are dynamically added to a workspace - right now, we give them an empty settings field, but this is not the correct behavior. The VS Code extension should support updating the client settings for a workspace, and the server should listen for client settings updates.

Dynamic client settings would also enable us to persist the server across settings changes without reloading.

---

_Label `server` added by @snowsignal on 2024-04-05 23:26_

---

_Assigned to @snowsignal by @snowsignal on 2024-04-09 21:25_

---

_Comment by @snowsignal on 2024-06-17 16:31_

Closing in favor of https://github.com/astral-sh/ruff/issues/11897

---

_Closed by @snowsignal on 2024-06-17 16:31_

---

_Closed by @snowsignal on 2024-06-17 16:31_

---
