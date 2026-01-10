```yaml
number: 8893
title: "Add `uv tree --outdated`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/tree-outdated
created_at: 2024-11-07T17:52:22Z
updated_at: 2024-11-07T20:10:48Z
url: https://github.com/astral-sh/uv/pull/8893
synced_at: 2026-01-10T12:00:00Z
```

# Add `uv tree --outdated`

---

_Pull request opened by @charliermarsh on 2024-11-07 17:52_

## Summary

Similar to `pip list --outdated`, but for `uv tree`.

## Test Plan

Looks like:

```
foo v0.1.0
└── flask v2.0.0 (latest: v3.0.3)
    ├── click v8.1.7
    ├── itsdangerous v2.2.0
    ├── jinja2 v3.1.4
    │   └── markupsafe v3.0.2
    └── werkzeug v3.1.2
        └── markupsafe v3.0.2
```

With `(latest: v3.0.3)` in bold cyan.


---

_Comment by @zanieb on 2024-11-07 17:53_

I presume you'll add a test :)

---

_Comment by @charliermarsh on 2024-11-07 17:55_

Do you think we should _only_ show outdated packages?

---

_Review requested from @zanieb by @charliermarsh on 2024-11-07 18:15_

---

_Review requested from @konstin by @charliermarsh on 2024-11-07 18:15_

---

_Label `enhancement` added by @charliermarsh on 2024-11-07 18:15_

---

_Comment by @zanieb on 2024-11-07 18:28_

Probably? I guess I'd be surprised to see packages that aren't outdated, but we need to anyway to display the tree?

---

_Comment by @charliermarsh on 2024-11-07 18:32_

We could basically limit the tree to outdated packages.

---

_Comment by @charliermarsh on 2024-11-07 18:33_

But it would often be just a list of packages since they may not be connected.

---

_Comment by @zanieb on 2024-11-07 18:36_

Let's just start with showing everything. Maybe bold the outdated ones?

---

_Comment by @zanieb on 2024-11-07 18:36_

I see it's already bold

---

_Comment by @charliermarsh on 2024-11-07 18:37_

Yeah it's bold and blue.

---

_@zanieb approved on 2024-11-07 20:08_

---

_Merged by @zanieb on 2024-11-07 20:10_

---

_Closed by @zanieb on 2024-11-07 20:10_

---

_Branch deleted on 2024-11-07 20:10_

---
