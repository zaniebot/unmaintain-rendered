```yaml
number: 4504
title: "Add support for `--reinstall` and `--reinstall-package` in `uv tool install`"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/tool-install-reinstall
created_at: 2024-06-25T01:17:46Z
updated_at: 2024-06-26T20:23:36Z
url: https://github.com/astral-sh/uv/pull/4504
synced_at: 2026-01-10T13:48:28Z
```

# Add support for `--reinstall` and `--reinstall-package` in `uv tool install`

---

_Pull request opened by @zanieb on 2024-06-25 01:17_

Adds support for `--reinstall` and `--reinstall-package` to `uv tool install`. These are already available via the installer settings, we just respect them now.

`--reinstall` implies a recreation of the environment and reinstallation of the entry points.
`--reinstall-package` will only update a subset of the environment. If the target package is the one with the entry points, we'll reinstall the entry points. Otherwise, the entry points are not changed.

---

_Label `preview` added by @zanieb on 2024-06-25 01:17_

---

_Marked ready for review by @zanieb on 2024-06-26 16:58_

---

_Review requested from @charliermarsh by @zanieb on 2024-06-26 19:59_

---

_@charliermarsh reviewed on 2024-06-26 20:03_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:72 on 2024-06-26 20:03_

Lets omit the periods from user-facing copy for consistency

---

_@charliermarsh reviewed on 2024-06-26 20:04_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:216 on 2024-06-26 20:04_

Should we put this on one line if multiple? Like `Installed: black, blackd`

---

_@charliermarsh approved on 2024-06-26 20:04_

---

_@zanieb reviewed on 2024-06-26 20:06_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:72 on 2024-06-26 20:06_

👍 

---

_@zanieb reviewed on 2024-06-26 20:07_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:216 on 2024-06-26 20:07_

I'm not sure. I kind of like that more here. I guess it depends if we'll include additional diagnostics about the installed entry points later.

---

_Merged by @zanieb on 2024-06-26 20:23_

---

_Closed by @zanieb on 2024-06-26 20:23_

---

_Branch deleted on 2024-06-26 20:23_

---
