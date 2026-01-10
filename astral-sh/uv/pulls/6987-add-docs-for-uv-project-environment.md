```yaml
number: 6987
title: "Add docs for `UV_PROJECT_ENVIRONMENT`"
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/docs-project-environment
created_at: 2024-09-03T23:13:39Z
updated_at: 2024-09-03T23:47:07Z
url: https://github.com/astral-sh/uv/pull/6987
synced_at: 2026-01-10T12:53:37Z
```

# Add docs for `UV_PROJECT_ENVIRONMENT`

---

_Pull request opened by @zanieb on 2024-09-03 23:13_

_No description provided._

---

_Label `documentation` added by @zanieb on 2024-09-03 23:13_

---

_Renamed from "Add docs for `UV_PROJECT_ENVIROMENT`" to "Add docs for `UV_PROJECT_ENVIRONMENT`" by @zanieb on 2024-09-03 23:24_

---

_@charliermarsh reviewed on 2024-09-03 23:35_

---

_Review comment by @charliermarsh on `docs/concepts/projects.md`:283 on 2024-09-03 23:35_

"path to the project vitrual environment"?

---

_@charliermarsh reviewed on 2024-09-03 23:35_

---

_Review comment by @charliermarsh on `docs/concepts/projects.md`:283 on 2024-09-03 23:35_

Mention the default?

---

_@charliermarsh reviewed on 2024-09-03 23:36_

---

_Review comment by @charliermarsh on `docs/concepts/projects.md`:290 on 2024-09-03 23:36_

```suggestion
This option can be used to write to the system Python environment, though it is not recommended.
`uv sync` will remove extraneous packages from the environment by default and, as such, may
leave the system in a broken state.
```

---

_@charliermarsh reviewed on 2024-09-03 23:37_

---

_Review comment by @charliermarsh on `docs/guides/integration/github.md`:236 on 2024-09-03 23:37_

For some reason the syntax highlighting here is different (dark rather than light blue) so maybe just sanity check that it's being rendered correctly.

---

_@charliermarsh approved on 2024-09-03 23:37_

---

_@zanieb reviewed on 2024-09-03 23:43_

---

_Review comment by @zanieb on `docs/guides/integration/github.md`:236 on 2024-09-03 23:43_

👍 looks good rendered

<img width="836" alt="Screenshot 2024-09-03 at 6 43 17 PM" src="https://github.com/user-attachments/assets/f757e945-960a-4393-bedc-dc759b557031">


---

_Merged by @charliermarsh on 2024-09-03 23:47_

---

_Closed by @charliermarsh on 2024-09-03 23:47_

---

_Branch deleted on 2024-09-03 23:47_

---
