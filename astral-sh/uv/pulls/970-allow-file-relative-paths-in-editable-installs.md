```yaml
number: 970
title: "Allow file:-relative paths in editable installs"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/file
created_at: 2024-01-18T21:07:54Z
updated_at: 2024-01-18T21:15:43Z
url: https://github.com/astral-sh/uv/pull/970
synced_at: 2026-01-12T16:04:20Z
```

# Allow file:-relative paths in editable installs

---

_@charliermarsh_

Supports editable install via (e.g.) `puffin pip install -e  file:.`, which pip seems to support.

Closes #964.

---

_Label `bug` added by @charliermarsh on 2024-01-18 21:07_

---

_@charliermarsh reviewed on 2024-01-18 21:08_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:182 on 2024-01-18 21:08_

I'm going to clean this up a bit separately, since the path-based versions also need to support env vars just like in `VerbatimUrl`.

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:182 on 2024-01-18 21:08_

And I want a `VerbatimPath`.

---

_@charliermarsh reviewed on 2024-01-18 21:08_

---

_Merged by @charliermarsh on 2024-01-18 21:15_

---

_Closed by @charliermarsh on 2024-01-18 21:15_

---

_Branch deleted on 2024-01-18 21:15_

---
