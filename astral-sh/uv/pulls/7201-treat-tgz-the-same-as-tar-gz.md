```yaml
number: 7201
title: treat .tgz the same as .tar.gz
type: pull_request
state: merged
author: soof-golan
labels:
  - compatibility
assignees: []
merged: true
base: main
head: soof-7081-tgz-source-dist
created_at: 2024-09-08T22:22:50Z
updated_at: 2024-09-08T23:09:40Z
url: https://github.com/astral-sh/uv/pull/7201
synced_at: 2026-01-12T16:07:44Z
```

# treat .tgz the same as .tar.gz

---

_@soof-golan_


## Summary

Fixes #7081 

Treats source distribution `.tgz` the same as `.tar.gz` plans

## Test Plan

Quick Version

```bash
cd $(mktemp -d)
uv init
uv add --dev build
.venv/bin/python -m build -s .
mv -v dist/*tar.gz dist/"$(basename dist/*.tar.gz .tar.gz)".tgz
uv pip install dist/*.tgz
```

Can add a proper test to the branch if requested


---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-08 22:26_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-09-08 22:26_

---

_Comment by @soof-golan on 2024-09-08 22:29_

~Still need to update snapshots...~
Edit: done

---

_@charliermarsh approved on 2024-09-08 23:03_

Thanks!

---

_Label `compatibility` added by @charliermarsh on 2024-09-08 23:03_

---

_Merged by @charliermarsh on 2024-09-08 23:09_

---

_Closed by @charliermarsh on 2024-09-08 23:09_

---
