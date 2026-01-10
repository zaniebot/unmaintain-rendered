```yaml
number: 10768
title: "Add a GitHub repository struct to `uv-git`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/github
created_at: 2025-01-20T01:38:40Z
updated_at: 2025-01-20T14:39:07Z
url: https://github.com/astral-sh/uv/pull/10768
synced_at: 2026-01-10T11:45:09Z
```

# Add a GitHub repository struct to `uv-git`

---

_Pull request opened by @charliermarsh on 2025-01-20 01:38_

## Summary

This is useful for https://github.com/astral-sh/uv/pull/10765, but we already have one usage today, so carving it out into a standalone PR.


---

_Label `internal` added by @charliermarsh on 2025-01-20 01:38_

---

_@charliermarsh reviewed on 2025-01-20 01:43_

---

_Review comment by @charliermarsh on `crates/uv-git/src/git.rs`:815 on 2025-01-20 01:43_

This is in the wrong place. 400-level errors are raised by `response.error_for_status_ref()?`!

---

_@charliermarsh reviewed on 2025-01-20 01:43_

---

_Review comment by @charliermarsh on `crates/uv-git/src/github.rs`:11 on 2025-01-20 01:43_

Uses the terminology of the GitHub API.

---

_Review comment by @konstin on `crates/uv-git/src/github.rs`:17 on 2025-01-20 08:45_

```suggestion
    /// Expects to receive a URL of the form: `https://github.com/{user}/{repo}[.git]`, e.g.,
```

---

_@konstin approved on 2025-01-20 08:46_

---

_Merged by @charliermarsh on 2025-01-20 14:39_

---

_Closed by @charliermarsh on 2025-01-20 14:39_

---

_Branch deleted on 2025-01-20 14:39_

---
