```yaml
number: 4948
title: Add support for any Python requests
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/python-request-any
created_at: 2024-07-09T22:46:07Z
updated_at: 2024-07-10T15:20:08Z
url: https://github.com/astral-sh/uv/pull/4948
synced_at: 2026-01-12T16:06:33Z
```

# Add support for any Python requests

---

_@zanieb_

For roundtrip in #4949 — it should also be fine to request `any` but the user can't construct it right now.

---

_Label `enhancement` added by @zanieb on 2024-07-09 22:46_

---

_Marked ready for review by @zanieb on 2024-07-09 23:19_

---

_Comment by @charliermarsh on 2024-07-10 00:27_

I find it strange that `uv python uninstall any` would install all though.

---

_Comment by @zanieb on 2024-07-10 01:03_

Yeah kind of. It seems weird to special case though? It's "uninstall all Python versions that match the query", e.g. `uv python uninstall >=3.10`. Anyhow, `uv pip install --python any ...` doesn't seem problematic? We can move slowly on this if you want, split it out of #4949 for this purpose.

---

_@charliermarsh approved on 2024-07-10 15:08_

---

_Merged by @zanieb on 2024-07-10 15:20_

---

_Closed by @zanieb on 2024-07-10 15:20_

---

_Branch deleted on 2024-07-10 15:20_

---
