```yaml
number: 5923
title: Ignore local configuration in tool commands
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
  - preview
assignees: []
merged: true
base: main
head: charlie/tool
created_at: 2024-08-08T17:50:02Z
updated_at: 2024-08-08T18:25:14Z
url: https://github.com/astral-sh/uv/pull/5923
synced_at: 2026-01-12T16:07:06Z
```

# Ignore local configuration in tool commands

---

_@charliermarsh_

## Summary

If you're running a user-level command, we shouldn't respect the local `pyproject.toml` or `uv.toml`.


---

_Review requested from @zanieb by @charliermarsh on 2024-08-08 17:50_

---

_Label `configuration` added by @charliermarsh on 2024-08-08 17:50_

---

_Label `preview` added by @charliermarsh on 2024-08-08 17:50_

---

_@zanieb reviewed on 2024-08-08 18:02_

---

_Review comment by @zanieb on `docs/configuration/files.md`:10 on 2024-08-08 18:02_

Minor problem here, `uv python install` without arguments reads from `.python-version` (and in the future `pyproject.toml`), it might need to respect project settings.

---

_@charliermarsh reviewed on 2024-08-08 18:06_

---

_Review comment by @charliermarsh on `docs/configuration/files.md`:10 on 2024-08-08 18:06_

Ok, I can back that part out?

---

_@zanieb reviewed on 2024-08-08 18:11_

---

_Review comment by @zanieb on `docs/configuration/files.md`:10 on 2024-08-08 18:11_

Technically it should probably ignore project settings unless implicitly installing versions for that project, but that seems complicated to explain... I guess I'd back it out entirely for now?

---

_Review requested from @zanieb by @charliermarsh on 2024-08-08 18:17_

---

_@zanieb approved on 2024-08-08 18:24_

---

_Merged by @charliermarsh on 2024-08-08 18:25_

---

_Closed by @charliermarsh on 2024-08-08 18:25_

---

_Branch deleted on 2024-08-08 18:25_

---
