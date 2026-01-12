```yaml
number: 16965
title: "Clarify `--project` flag help text to indicate project discovery"
type: pull_request
state: merged
author: nooscraft
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-project-flag-help-text
created_at: 2025-12-03T13:54:44Z
updated_at: 2025-12-03T18:15:36Z
url: https://github.com/astral-sh/uv/pull/16965
synced_at: 2026-01-12T16:12:32Z
```

# Clarify `--project` flag help text to indicate project discovery

---

_@nooscraft_

Clarify `--project` flag help text to indicate project discovery

  Update the help text for `--project` from "Run the command within
  the given project directory" to "Discover a project in the given
  directory" to better reflect its actual behavior.

  The `--project` flag affects file discovery (pyproject.toml, uv.toml,
  etc.) but does not change the working directory. Users should use
  `--directory` for changing the working directory.

  Fixes #16718

---

_Comment by @zanieb on 2025-12-03 13:57_

Will require `cargo dev generate-all`

---

_Label `documentation` added by @zanieb on 2025-12-03 14:08_

---

_@zanieb approved on 2025-12-03 14:08_

---

_Merged by @zanieb on 2025-12-03 18:15_

---

_Closed by @zanieb on 2025-12-03 18:15_

---
