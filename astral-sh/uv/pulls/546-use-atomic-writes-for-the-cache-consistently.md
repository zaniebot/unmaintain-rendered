```yaml
number: 546
title: Use atomic writes for the cache consistently
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konstin/atomic-writes
created_at: 2023-12-04T16:07:00Z
updated_at: 2023-12-04T17:02:02Z
url: https://github.com/astral-sh/uv/pull/546
synced_at: 2026-01-12T16:04:01Z
```

# Use atomic writes for the cache consistently

---

_@konstin_

Ensure we're using atomic writes everywhere in our cache to avoid broken cache records and error with parallel puffin actions (https://github.com/astral-sh/puffin/pull/544#issuecomment-1838841581).

All json files that are written to the cache are written atomically and the build wheels are written to temp dir and then moved atomically. I didn't touch venv creation though, i don't think that's worth it since python does not support atomic package installation through its design.

---

_@konstin reviewed on 2023-12-04 16:07_

---

_Review comment by @konstin on `crates/puffin-cli/src/commands/venv.rs`:94 on 2023-12-04 16:07_

gourgeist handles this more correctly internally by only removing a directory if it was a venv before.

---

_@charliermarsh reviewed on 2023-12-04 16:59_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/lib.rs`:355 on 2023-12-04 16:59_

What if `path` already exists? What happens?

---

_@charliermarsh reviewed on 2023-12-04 17:01_

---

_Review comment by @charliermarsh on `crates/puffin-build/src/lib.rs`:426 on 2023-12-04 17:01_

This could still error, right? Or no, because `rename` only fails if the target is an existing _directory_?

---

_@charliermarsh approved on 2023-12-04 17:01_

---

_@charliermarsh reviewed on 2023-12-04 17:01_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/lib.rs`:355 on 2023-12-04 17:01_

Oh, these are individual files, not directories. Seems fine.

---

_Merged by @charliermarsh on 2023-12-04 17:02_

---

_Closed by @charliermarsh on 2023-12-04 17:02_

---

_Branch deleted on 2023-12-04 17:02_

---
