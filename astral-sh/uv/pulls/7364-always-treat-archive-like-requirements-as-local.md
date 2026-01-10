```yaml
number: 7364
title: Always treat archive-like requirements as local files
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/filename-package
created_at: 2024-09-13T15:38:42Z
updated_at: 2024-09-13T16:01:26Z
url: https://github.com/astral-sh/uv/pull/7364
synced_at: 2026-01-10T12:53:45Z
```

# Always treat archive-like requirements as local files

---

_Pull request opened by @charliermarsh on 2024-09-13 15:38_

## Summary

`uv pip install foo.tar.gz` will now always treat `foo.tar.gz` as a local file. This matches pip's behavior.

Closes https://github.com/astral-sh/uv/issues/7309.


---

_Label `bug` added by @charliermarsh on 2024-09-13 15:38_

---

_Review requested from @konstin by @charliermarsh on 2024-09-13 15:38_

---

_Marked ready for review by @charliermarsh on 2024-09-13 15:38_

---

_Review comment by @konstin on `crates/pep508-rs/src/lib.rs`:699 on 2024-09-13 15:45_

Should we use the `DistFilename` parsing here or are you intentionally using a wider filter?

---

_@konstin approved on 2024-09-13 15:45_

---

_@charliermarsh reviewed on 2024-09-13 15:46_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/lib.rs`:699 on 2024-09-13 15:46_

Intentionally a bit wider to match pip.

---

_Merged by @charliermarsh on 2024-09-13 16:01_

---

_Closed by @charliermarsh on 2024-09-13 16:01_

---

_Branch deleted on 2024-09-13 16:01_

---
