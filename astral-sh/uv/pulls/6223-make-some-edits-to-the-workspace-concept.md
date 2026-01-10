```yaml
number: 6223
title: Make some edits to the workspace concept documentation
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/workspace-docs
created_at: 2024-08-19T18:38:25Z
updated_at: 2024-08-19T18:57:32Z
url: https://github.com/astral-sh/uv/pull/6223
synced_at: 2026-01-10T13:09:51Z
```

# Make some edits to the workspace concept documentation

---

_Pull request opened by @charliermarsh on 2024-08-19 18:38_

_No description provided._

---

_Review requested from @zanieb by @charliermarsh on 2024-08-19 18:38_

---

_Label `documentation` added by @charliermarsh on 2024-08-19 18:38_

---

_@zanieb reviewed on 2024-08-19 18:43_

---

_Review comment by @zanieb on `docs/concepts/workspaces.md`:12 on 2024-08-19 18:43_

This seems vaguely conflicting with the above

> distinct packages with independent dependencies.

Maybe we should say "independent requirements" above? or "consistent set of dependency versions" here? I'm not sure.

---

_@zanieb reviewed on 2024-08-19 18:44_

---

_Review comment by @zanieb on `docs/concepts/workspaces.md`:16 on 2024-08-19 18:44_

Perhaps "in a particular workspace member"?

---

_@zanieb reviewed on 2024-08-19 18:44_

---

_Review comment by @zanieb on `docs/concepts/workspaces.md`:20 on 2024-08-19 18:44_

We could also suggest in a !!! tip that `uv init child` in a project will do this for you.

---

_@zanieb reviewed on 2024-08-19 18:45_

---

_Review comment by @zanieb on `docs/concepts/workspaces.md`:37 on 2024-08-19 18:45_

If they do not, what happens? Do we ignore them? Do we fail?

---

_@zanieb reviewed on 2024-08-19 18:47_

---

_Review comment by @zanieb on `docs/concepts/workspaces.md`:95 on 2024-08-19 18:47_

We should link to the dependency sources concept document.

---

_@zanieb approved on 2024-08-19 18:47_

Thank you!

---

_Merged by @charliermarsh on 2024-08-19 18:57_

---

_Closed by @charliermarsh on 2024-08-19 18:57_

---

_Branch deleted on 2024-08-19 18:57_

---
