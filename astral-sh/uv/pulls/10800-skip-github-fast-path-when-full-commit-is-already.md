```yaml
number: 10800
title: Skip GitHub fast path when full commit is already known
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/full-commit
created_at: 2025-01-21T01:40:33Z
updated_at: 2025-01-21T19:17:52Z
url: https://github.com/astral-sh/uv/pull/10800
synced_at: 2026-01-10T11:45:11Z
```

# Skip GitHub fast path when full commit is already known

---

_Pull request opened by @charliermarsh on 2025-01-21 01:40_

## Summary

If we have a `GitReference::FullCommit`, we don't need to go to GitHub to resolve the SHA. We already have it!


---

_Label `performance` added by @charliermarsh on 2025-01-21 01:40_

---

_Converted to draft by @charliermarsh on 2025-01-21 01:48_

---

_Review requested from @konstin by @charliermarsh on 2025-01-21 02:12_

---

_Review requested from @ibraheemdev by @charliermarsh on 2025-01-21 02:14_

---

_Marked ready for review by @charliermarsh on 2025-01-21 02:14_

---

_@zanieb reviewed on 2025-01-21 02:21_

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:77 on 2025-01-21 02:21_

Is this change separate from the behavioral change of this pull request?

---

_@zanieb reviewed on 2025-01-21 02:22_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:1754 on 2025-01-21 02:22_

This error change is a bit of a bummer.

---

_@charliermarsh reviewed on 2025-01-21 02:29_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_install.rs`:1754 on 2025-01-21 02:29_

Is it actually worse?

---

_@charliermarsh reviewed on 2025-01-21 02:29_

---

_Review comment by @charliermarsh on `crates/uv-git/src/git.rs`:77 on 2025-01-21 02:29_

No, this is necessary. `Self::FullCommit` must be a commit. It's not enough to store a thing that looks like a commit.

---

_@konstin approved on 2025-01-21 08:09_

---

_@ibraheemdev approved on 2025-01-21 15:13_

---

_@zanieb reviewed on 2025-01-21 15:25_

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:77 on 2025-01-21 15:25_

I thought it was guaranteed that a 40 character revision is a commit? Or is the problem that's GitHub only?

---

_@zanieb reviewed on 2025-01-21 15:26_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:1754 on 2025-01-21 15:26_

Well it wasn't great before, but... yeah I think it's worse.

---

_@charliermarsh reviewed on 2025-01-21 15:56_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_install.rs`:1754 on 2025-01-21 15:56_

I can add some special-casing if you want.

---

_@zanieb reviewed on 2025-01-21 16:16_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:1754 on 2025-01-21 16:16_

I think it's fine as a follow-up, if you want to just open an issue to track it.

---

_@charliermarsh reviewed on 2025-01-21 17:25_

---

_Review comment by @charliermarsh on `crates/uv-git/src/git.rs`:77 on 2025-01-21 17:25_

It's specific to GitHub. Git itself allows it. We could continue to make this assumption if you're comfortable with it? (We were already fetching this commit from GitHub before so removing this isn't a performance regression; it's just "the same" as before.)

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:77 on 2025-01-21 17:42_

I'm comfortable with assuming it, or not. I was mostly just confused that it changed here.

---

_@zanieb reviewed on 2025-01-21 17:42_

---

_Merged by @charliermarsh on 2025-01-21 19:17_

---

_Closed by @charliermarsh on 2025-01-21 19:17_

---

_Branch deleted on 2025-01-21 19:17_

---
