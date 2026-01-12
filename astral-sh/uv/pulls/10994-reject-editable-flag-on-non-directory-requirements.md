```yaml
number: 10994
title: "Reject `--editable` flag on non-directory requirements"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ed
created_at: 2025-01-27T17:46:45Z
updated_at: 2025-01-27T19:37:24Z
url: https://github.com/astral-sh/uv/pull/10994
synced_at: 2026-01-12T16:09:37Z
```

# Reject `--editable` flag on non-directory requirements

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/10992.


---

_Label `bug` added by @charliermarsh on 2025-01-27 17:46_

---

_Review requested from @zanieb by @charliermarsh on 2025-01-27 17:47_

---

_@charliermarsh reviewed on 2025-01-27 17:48_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/edit.rs`:2221 on 2025-01-27 17:48_

This is a workspace member, passing `--editable` is just ignored. Should we _not_ error here?

---

_@zanieb reviewed on 2025-01-27 17:51_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:1315 on 2025-01-27 17:51_

I think this should also say something like "Editable installs can only be used with local directories"? It's not entirely obvious.

---

_@zanieb reviewed on 2025-01-27 17:52_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:2221 on 2025-01-27 17:52_

I think if an option is redundant then it makes sense not to error, right?

This change is because we error here now?

---

_@charliermarsh reviewed on 2025-01-27 17:58_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/pyproject.rs`:1315 on 2025-01-27 17:58_

Changed.

---

_@charliermarsh reviewed on 2025-01-27 17:58_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/edit.rs`:2221 on 2025-01-27 17:58_

Yeah. I mean it's a bit weird. `--editable` and `--no-editable` are flags on `uv sync` for workspaces, but they have no effect here. It's not a persistent setting.

---

_Review requested from @zanieb by @charliermarsh on 2025-01-27 17:59_

---

_@charliermarsh reviewed on 2025-01-27 17:59_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/edit.rs`:2221 on 2025-01-27 17:59_

We could error if they pass `--no-editable`, because it won't have the intended effect.

---

_@zanieb reviewed on 2025-01-27 18:22_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:2221 on 2025-01-27 18:22_

Can't workspace members have `editable = true/false` in their source definition?

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:2221 on 2025-01-27 18:23_

Nevermind, that's not the case. Yeah I'd expect an error on `--no-editable`.

---

_@zanieb reviewed on 2025-01-27 18:23_

---

_@zanieb approved on 2025-01-27 18:23_

---

_Merged by @charliermarsh on 2025-01-27 19:37_

---

_Closed by @charliermarsh on 2025-01-27 19:37_

---

_Branch deleted on 2025-01-27 19:37_

---
