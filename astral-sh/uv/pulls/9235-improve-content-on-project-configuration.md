```yaml
number: 9235
title: Improve content on project configuration
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/project-config
created_at: 2024-11-19T19:06:32Z
updated_at: 2024-11-21T00:31:04Z
url: https://github.com/astral-sh/uv/pull/9235
synced_at: 2026-01-10T12:00:00Z
```

# Improve content on project configuration

---

_Pull request opened by @zanieb on 2024-11-19 19:06_

_No description provided._

---

_Label `documentation` added by @zanieb on 2024-11-19 22:18_

---

_Review comment by @konstin on `docs/concepts/projects/layout.md`:20 on 2024-11-20 08:58_

Do we need the description here?

---

_@konstin approved on 2024-11-20 08:58_

---

_@zanieb reviewed on 2024-11-20 14:49_

---

_Review comment by @zanieb on `docs/concepts/projects/layout.md`:20 on 2024-11-20 14:49_

Nope!

---

_Merged by @zanieb on 2024-11-20 14:49_

---

_Closed by @zanieb on 2024-11-20 14:49_

---

_Branch deleted on 2024-11-20 14:49_

---

_@jooon reviewed on 2024-11-20 23:36_

---

_Review comment by @jooon on `docs/concepts/projects/config.md`:12 on 2024-11-20 23:36_

This snippet out of context looks like a minimal valid pyproject.toml, but it is not because the [project] section requires name and version. The requires-python line worked by itself before in layout.md when it was just under the minimal config. Here however, I would add them back:

```
[project]
name = "example"
version = "0.1.0"
requires-python = ">=3.12"
```

---

_@zanieb reviewed on 2024-11-21 00:31_

---

_Review comment by @zanieb on `docs/concepts/projects/config.md`:12 on 2024-11-21 00:31_

Thanks!

---
