```yaml
number: 5801
title: "Add `--no-sources` to avoid reading from `tool.uv.sources`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/no-sources
created_at: 2024-08-05T20:31:37Z
updated_at: 2024-08-06T14:14:20Z
url: https://github.com/astral-sh/uv/pull/5801
synced_at: 2026-01-12T16:07:01Z
```

# Add `--no-sources` to avoid reading from `tool.uv.sources`

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/5791.


---

_Label `cli` added by @charliermarsh on 2024-08-05 20:31_

---

_Label `preview` added by @charliermarsh on 2024-08-05 20:31_

---

_Review requested from @zanieb by @charliermarsh on 2024-08-05 20:57_

---

_Review requested from @konstin by @charliermarsh on 2024-08-05 20:57_

---

_Comment by @zanieb on 2024-08-05 21:11_

Want to document this in `docs/concepts/dependencies.md`?

---

_Comment by @charliermarsh on 2024-08-05 21:17_

Sure.

---

_Review request for @zanieb removed by @charliermarsh on 2024-08-06 02:54_

---

_Review requested from @zanieb by @charliermarsh on 2024-08-06 02:54_

---

_Review comment by @konstin on `docs/concepts/dependencies.md`:79 on 2024-08-06 08:05_

This should mention we also ignore workspace, i.e. we behave as if you were using the project as a path dep from another tool that doesn't speak uv

---

_@konstin approved on 2024-08-06 08:05_

---

_Merged by @charliermarsh on 2024-08-06 14:14_

---

_Closed by @charliermarsh on 2024-08-06 14:14_

---

_Branch deleted on 2024-08-06 14:14_

---
