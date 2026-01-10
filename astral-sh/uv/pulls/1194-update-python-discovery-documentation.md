```yaml
number: 1194
title: Update Python discovery documentation
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/venv
created_at: 2024-01-30T21:32:20Z
updated_at: 2024-01-31T15:42:33Z
url: https://github.com/astral-sh/uv/pull/1194
synced_at: 2026-01-10T15:39:03Z
```

# Update Python discovery documentation

---

_Pull request opened by @charliermarsh on 2024-01-30 21:32_

Closes https://github.com/astral-sh/puffin/issues/1109.

---

_Marked ready for review by @charliermarsh on 2024-01-30 21:32_

---

_Review requested from @zanieb by @charliermarsh on 2024-01-30 21:32_

---

_Review requested from @konstin by @charliermarsh on 2024-01-30 21:32_

---

_Label `documentation` added by @charliermarsh on 2024-01-30 21:32_

---

_Review comment by @konstin on `README.md`:119 on 2024-01-31 12:08_

While this technically works, i consider it an intentionally undocumented implementation detail an would recommend user to always activate the venv through the activator scripts we install instead.

---

_Review comment by @konstin on `README.md`:131 on 2024-01-31 12:11_

We want to switch this to reading the registry ourselves

```suggestion
  will use the same mechanism as `py --list-paths` to discover Python interpreters, and will select the
```

---

_@konstin approved on 2024-01-31 12:11_

---

_Review comment by @charliermarsh on `README.md`:119 on 2024-01-31 15:24_

How else would you recommend installing into another virtualenv?

---

_@charliermarsh reviewed on 2024-01-31 15:24_

---

_@konstin reviewed on 2024-01-31 15:33_

---

_Review comment by @konstin on `README.md`:119 on 2024-01-31 15:33_

I would recommend installing puffin globally (standalone installers, pipx), installing puffin in the other venv too or activating the other venv and using `.puffin-venv/bin/puffin`. It's not that bad (`VIRTUAL_ENV` is too entrenched to changed), it's more a preference of not exposing users to the internals.

---

_@charliermarsh reviewed on 2024-01-31 15:39_

---

_Review comment by @charliermarsh on `README.md`:119 on 2024-01-31 15:39_

I'm gonna leave it for now as an example of what's possible but I'll move it lower.

---

_Merged by @charliermarsh on 2024-01-31 15:42_

---

_Closed by @charliermarsh on 2024-01-31 15:42_

---

_Branch deleted on 2024-01-31 15:42_

---
