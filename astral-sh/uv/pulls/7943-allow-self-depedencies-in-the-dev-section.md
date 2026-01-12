```yaml
number: 7943
title: "Allow self-depedencies in the `dev` section"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-dev-recursive
created_at: 2024-10-05T15:46:14Z
updated_at: 2024-10-06T14:42:57Z
url: https://github.com/astral-sh/uv/pull/7943
synced_at: 2026-01-12T16:08:05Z
```

# Allow self-depedencies in the `dev` section

---

_@zanieb_

https://github.com/astral-sh/uv/pull/7766 banned using `uv add` to create self-dependencies in the `dev` section which breaks `uv add --dev .[extra]` which is a fair use-case for adding a self-dependency.

Maybe we should only allow this if the added requirement includes an extra group? Otherwise it's a bit weird.

---

_Label `bug` added by @zanieb on 2024-10-05 15:46_

---

_Review requested from @charliermarsh by @zanieb on 2024-10-05 15:47_

---

_@charliermarsh approved on 2024-10-05 16:53_

Nit: that PR didn’t ban self dependencies in dev entirely; just adding them via uv add.

---

_Merged by @zanieb on 2024-10-06 14:42_

---

_Closed by @zanieb on 2024-10-06 14:42_

---

_Branch deleted on 2024-10-06 14:42_

---
