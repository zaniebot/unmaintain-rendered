```yaml
number: 296
title: Add support for Git revision references in VCS URLs
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-11-02T18:12:45Z
updated_at: 2023-11-02T19:10:04Z
url: https://github.com/astral-sh/uv/issues/296
synced_at: 2026-01-12T15:58:22Z
```

# Add support for Git revision references in VCS URLs

---

_@charliermarsh_

See: https://pip.pypa.io/en/stable/topics/vcs-support. We need to support, e.g., `MyProject @ git+https://git.example.com/MyProject.git@v1.0`. I didn't realize that pip expects a different format here than Cargo, which uses query parameters.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-02 18:12_

---

_Label `bug` added by @charliermarsh on 2023-11-02 18:12_

---

_Added to milestone `Feature complete` by @charliermarsh on 2023-11-02 18:12_

---

_Closed by @charliermarsh on 2023-11-02 19:10_

---
