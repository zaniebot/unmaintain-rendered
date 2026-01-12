```yaml
number: 11785
title: "Handle nested .git directories in `uv init`"
type: pull_request
state: open
author: mason-fabel
labels: []
assignees: []
base: main
head: mf/11655_nested-dotgit-directories
created_at: 2025-02-26T01:15:33Z
updated_at: 2025-04-14T08:00:31Z
url: https://github.com/astral-sh/uv/pull/11785
synced_at: 2026-01-12T16:10:00Z
```

# Handle nested .git directories in `uv init`

---

_@mason-fabel_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
- Allows `uv init --vcs=git` when parent dir has .git dir
- Fixes some tests in when parent dir has .git dir
- Resolves #11655

## Test Plan
- Verified locally
- Passes github CI



---

_@mason-fabel reviewed on 2025-02-27 03:56_

---

_Review comment by @mason-fabel on `crates/uv/tests/it/common/mod.rs`:335 on 2025-02-27 03:56_

From https://github.com/astral-sh/uv/pull/11761

---

_Converted to draft by @mason-fabel on 2025-02-27 03:57_

---

_Marked ready for review by @mason-fabel on 2025-02-27 04:27_

---

_Review requested from @zanieb by @charliermarsh on 2025-02-28 03:00_

---
