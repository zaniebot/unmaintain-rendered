```yaml
number: 1428
title: "Ignore invalid extra named `.none`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/none
created_at: 2024-02-16T04:52:53Z
updated_at: 2024-02-16T05:01:22Z
url: https://github.com/astral-sh/uv/pull/1428
synced_at: 2026-01-12T16:04:37Z
```

# Ignore invalid extra named `.none`

---

_@charliermarsh_

## Summary

Some packages erroneously include an extra named `.none`. It turns out that certain versions of `flit` included this by accident: https://github.com/pypa/flit/issues/228/.

This PR adds leniency for that specific extra name.

Closes https://github.com/astral-sh/uv/issues/1363.

Closes https://github.com/astral-sh/uv/issues/1399.


---

_Label `bug` added by @charliermarsh on 2024-02-16 04:53_

---

_@zanieb approved on 2024-02-16 04:56_

---

_Comment by @zanieb on 2024-02-16 04:56_

We ought to write a scenario for this, can you add one to `extras.json` or open an issue to do so?

---

_Merged by @charliermarsh on 2024-02-16 05:01_

---

_Closed by @charliermarsh on 2024-02-16 05:01_

---

_Branch deleted on 2024-02-16 05:01_

---
