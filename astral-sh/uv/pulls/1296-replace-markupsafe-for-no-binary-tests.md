```yaml
number: 1296
title: Replace MarkupSafe for no-binary tests
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/anyio
created_at: 2024-02-13T15:08:41Z
updated_at: 2024-02-14T04:44:09Z
url: https://github.com/astral-sh/uv/pull/1296
synced_at: 2026-01-12T16:04:34Z
```

# Replace MarkupSafe for no-binary tests

---

_@charliermarsh_

Want to see if this makes a difference.

---

_Comment by @charliermarsh on 2024-02-13 15:13_

They're still pretty slow (like > 10s), but not > 30s...

---

_Comment by @zanieb on 2024-02-13 16:07_

I think we could drop these entirely if you wanted. Otherwise this seems good.

---

_Comment by @zanieb on 2024-02-13 17:42_

Note we discussed sync and there _is not_ scenario test coverage for `--no-binary` behavior with _cached_ packages so we cannot remove that test. The others have coverage.

---

_Marked ready for review by @charliermarsh on 2024-02-14 04:40_

---

_Label `internal` added by @charliermarsh on 2024-02-14 04:41_

---

_Comment by @charliermarsh on 2024-02-14 04:41_

Removes the others, and replaced the reinstall test with `anyio`.

---

_Merged by @charliermarsh on 2024-02-14 04:44_

---

_Closed by @charliermarsh on 2024-02-14 04:44_

---

_Branch deleted on 2024-02-14 04:44_

---
