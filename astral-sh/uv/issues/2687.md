```yaml
number: 2687
title: Activation prompt shows wrong path on windows and nushell
type: issue
state: closed
author: BenediktMaag
labels:
  - bug
assignees: []
created_at: 2024-03-27T07:49:26Z
updated_at: 2024-03-27T14:46:40Z
url: https://github.com/astral-sh/uv/issues/2687
synced_at: 2026-01-10T05:40:32Z
```

# Activation prompt shows wrong path on windows and nushell

---

_Issue opened by @BenediktMaag on 2024-03-27 07:49_

When creating an environment in nushell (uv venv) it tells me how to activate:

- Activate with: overlay use .venv\bin\activate.nu

Since I am on windows, the subfolder containing the activation files is called Scripts.
When creating the environment within powershell the prompt points to the right folder:

- Activate with: .venv\Scripts\activate

---

_Renamed from "Activation prompt show wrong path" to "Activation prompt shows wrong path on windows and nushell" by @BenediktMaag on 2024-03-27 07:49_

---

_Label `bug` added by @charliermarsh on 2024-03-27 14:22_

---

_Comment by @charliermarsh on 2024-03-27 14:22_

What's the "correct" invocation on nushell on Windows?

---

_Comment by @BenediktMaag on 2024-03-27 14:29_

overlay use .venv\Scripts\activate.nu

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-27 14:31_

---

_Closed by @charliermarsh on 2024-03-27 14:46_

---
