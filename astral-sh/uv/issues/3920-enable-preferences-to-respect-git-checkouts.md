```yaml
number: 3920
title: Enable preferences to respect Git checkouts
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
created_at: 2024-05-30T01:31:02Z
updated_at: 2024-06-01T12:40:13Z
url: https://github.com/astral-sh/uv/issues/3920
synced_at: 2026-01-12T15:58:46Z
```

# Enable preferences to respect Git checkouts

---

_@charliermarsh_

If the lockfile has a pinned Git commit, we should be able to reuse that commit during a subsequent resolution, if the requested revision is the same (and the user hasn't requested `--upgrade` or similar). We can't do this with `requirements.txt` because there's no way to determine the requested revision (only the resolved SHA); but in the lockfile, we have both.

---

_Label `enhancement` added by @charliermarsh on 2024-05-30 01:31_

---

_Label `preview` added by @charliermarsh on 2024-05-30 01:31_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-30 01:53_

---

_Closed by @charliermarsh on 2024-06-01 12:40_

---
