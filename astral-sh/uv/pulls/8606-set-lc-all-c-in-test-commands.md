```yaml
number: 8606
title: "Set `LC_ALL=C` in test commands"
type: pull_request
state: merged
author: vivienm
labels:
  - testing
assignees: []
merged: true
base: main
head: lc_all
created_at: 2024-10-27T10:41:38Z
updated_at: 2024-10-27T14:26:34Z
url: https://github.com/astral-sh/uv/pull/8606
synced_at: 2026-01-12T16:08:23Z
```

# Set `LC_ALL=C` in test commands

---

_@vivienm_

## Summary

The output of some programs used in uv tests depends on the OS language. In particular, git error messages are localized.

As a consequence, the following tests fail if run on a non-US environment:

```
pip_compile::git_source_missing_tag
pip_install::install_git_private_https_pat_not_authorized
pip_install::install_git_public_https_missing_branch_or_tag
pip_install::install_git_public_https_missing_commit
```

Example error:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: install_git_public_https_missing_branch_or_tag
Source: crates/uv/tests/it/pip_install.rs:1600
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    7     7 │   Caused by: failed to clone into: [CACHE_DIR]/git-v0/db/8dab139913c4b566
    8     8 │   Caused by: failed to fetch branch or tag `2.0.0`
    9     9 │   Caused by: process didn't exit successfully: `git fetch [...]` (exit code: 128)
   10    10 │ --- stderr
   11       │-fatal: couldn't find remote ref refs/tags/2.0.0
         11 │+fatal : impossible de trouver la référence distante refs/tags/2.0.0
────────────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```

On Unix-like platforms, a simple workaround is to set the environment variable `LC_ALL=C` (there are variants e.g. `LANG=en_US.UTF-8`, but they are likely less robust).

My proposal here is to set this variable directly in tests, to avoid developers having to configure their environment before running tests.

## Test Plan

Tests listed above no longer fail with this workaround.


---

_Comment by @zanieb on 2024-10-27 13:46_

Thanks! Can you add this to [`add_shared_args`](https://github.com/astral-sh/uv/blob/main/crates/uv/tests/it/common/mod.rs#L458) instead?

---

_Review requested from @konstin by @zanieb on 2024-10-27 13:47_

---

_Comment by @vivienm on 2024-10-27 14:17_

> Thanks! Can you add this to [`add_shared_args`](https://github.com/astral-sh/uv/blob/main/crates/uv/tests/it/common/mod.rs#L458) instead?

:+1: Done!

---

_Label `testing` added by @zanieb on 2024-10-27 14:17_

---

_@zanieb approved on 2024-10-27 14:17_

---

_Merged by @zanieb on 2024-10-27 14:24_

---

_Closed by @zanieb on 2024-10-27 14:24_

---

_Branch deleted on 2024-10-27 14:26_

---
