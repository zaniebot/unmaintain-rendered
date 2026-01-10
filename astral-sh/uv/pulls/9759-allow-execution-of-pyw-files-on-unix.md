```yaml
number: 9759
title: Allow execution of pyw files on Unix
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/pyw-unix
created_at: 2024-12-10T00:56:42Z
updated_at: 2024-12-10T02:52:05Z
url: https://github.com/astral-sh/uv/pull/9759
synced_at: 2026-01-10T12:00:01Z
```

# Allow execution of pyw files on Unix

---

_Pull request opened by @zanieb on 2024-12-10 00:56_

I don't see any real reason to forbid executing these in a cross-platform way

```
❯ echo "print('hello world')" > test.pyw
❯ uv run test.pyw
error: Failed to spawn: `test.pyw`
  Caused by: No such file or directory (os error 2)
❯ cargo run -q -- run test.pyw
hello world
```

Closes https://github.com/astral-sh/uv/issues/9757

---

_Label `enhancement` added by @zanieb on 2024-12-10 00:56_

---

_Review requested from @konstin by @zanieb on 2024-12-10 00:57_

---

_Marked ready for review by @zanieb on 2024-12-10 00:58_

---

_@charliermarsh approved on 2024-12-10 02:47_

---

_Merged by @charliermarsh on 2024-12-10 02:52_

---

_Closed by @charliermarsh on 2024-12-10 02:52_

---

_Branch deleted on 2024-12-10 02:52_

---
