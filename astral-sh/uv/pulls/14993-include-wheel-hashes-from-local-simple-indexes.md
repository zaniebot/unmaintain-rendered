```yaml
number: 14993
title: Include wheel hashes from local Simple indexes
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/path-hash
created_at: 2025-07-31T13:40:22Z
updated_at: 2025-07-31T14:20:50Z
url: https://github.com/astral-sh/uv/pull/14993
synced_at: 2026-01-12T16:11:31Z
```

# Include wheel hashes from local Simple indexes

---

_@charliermarsh_

## Summary

This just looks like an oversight. We weren't including hashes from local Simple API indexes if a package had both a wheel and a source distribution.

Closes https://github.com/astral-sh/uv/issues/14883


---

_Label `bug` added by @charliermarsh on 2025-07-31 13:40_

---

_Marked ready for review by @charliermarsh on 2025-07-31 13:40_

---

_@zanieb approved on 2025-07-31 13:44_

---

_Comment by @charliermarsh on 2025-07-31 13:50_

Sort of a pain to write a test for this but I really should.

---

_Renamed from "Include wheel hashes from local `--find-links` index" to "Include wheel hashes from local Simple indexes" by @charliermarsh on 2025-07-31 14:07_

---

_Comment by @charliermarsh on 2025-07-31 14:09_

Added a test, which also led me to realize this isn't `--find-links`, but local Simple API indexes. (The fix was right, I was just confused on the terminology.)

---

_@charliermarsh reviewed on 2025-07-31 14:12_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:10442 on 2025-07-31 14:12_

(Created with `uv init --lib`, no modifications.)

---

_Merged by @charliermarsh on 2025-07-31 14:20_

---

_Closed by @charliermarsh on 2025-07-31 14:20_

---

_Branch deleted on 2025-07-31 14:20_

---
