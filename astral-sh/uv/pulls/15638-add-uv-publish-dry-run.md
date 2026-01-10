```yaml
number: 15638
title: "Add `uv publish --dry-run`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/pyx-publish
created_at: 2025-09-02T18:59:41Z
updated_at: 2025-09-03T01:24:33Z
url: https://github.com/astral-sh/uv/pull/15638
synced_at: 2026-01-10T06:44:33Z
```

# Add `uv publish --dry-run`

---

_Pull request opened by @charliermarsh on 2025-09-02 18:59_

## Summary

`uv publish --dry-run` will perform the `--check-url` validation, and hit the `/validate` endpoint if the registry is known to support fast-path validation (like pyx). The `/validate` endpoint lets us validate an upload without uploading the file _contents_, which lets you skip the expensive step for common mistakes.

In the future, my hope is that the `/validate` step will deprecated in favor of Upload API 2.0.


---

_Review requested from @konstin by @charliermarsh on 2025-09-02 18:59_

---

_Review requested from @zanieb by @charliermarsh on 2025-09-02 18:59_

---

_Label `enhancement` added by @charliermarsh on 2025-09-02 18:59_

---

_Marked ready for review by @charliermarsh on 2025-09-02 18:59_

---

_@zanieb approved on 2025-09-02 19:39_

---

_Review comment by @zanieb on `crates/uv-publish/src/lib.rs`:518 on 2025-09-02 19:41_

We should probably fail if we can't load the authentication?

Does `--dry-run` do anything if this early-exits? I guess it runs `--check-url`? Should we log the case where validation doesn't run at least?

---

_@zanieb reviewed on 2025-09-02 19:41_

---

_Merged by @charliermarsh on 2025-09-03 01:24_

---

_Closed by @charliermarsh on 2025-09-03 01:24_

---

_Branch deleted on 2025-09-03 01:24_

---
