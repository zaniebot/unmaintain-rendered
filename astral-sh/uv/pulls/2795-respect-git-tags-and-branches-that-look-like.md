```yaml
number: 2795
title: Respect Git tags and branches that look like short commits
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ambiguous-git
created_at: 2024-04-03T03:13:26Z
updated_at: 2024-04-04T02:05:55Z
url: https://github.com/astral-sh/uv/pull/2795
synced_at: 2026-01-12T16:05:13Z
```

# Respect Git tags and branches that look like short commits

---

_@charliermarsh_

## Summary

If we're given a Git reference like `20240222`, we currently treat it as a short commit hash. However... it _could_ be a branch or a tag. This PR improves the Git reference logic to ensure that ambiguous references like `20240222` are handled appropriately, by attempting to extract it as a branch, then a tag, then a short commit hash.

Closes https://github.com/astral-sh/uv/issues/2772.


---

_Label `bug` added by @charliermarsh on 2024-04-03 03:19_

---

_Marked ready for review by @charliermarsh on 2024-04-03 04:02_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-04-03 04:04_

---

_Review requested from @zanieb by @charliermarsh on 2024-04-03 04:04_

---

_@charliermarsh reviewed on 2024-04-03 23:15_

---

_Review comment by @charliermarsh on `crates/uv-git/src/git.rs`:33 on 2024-04-03 23:15_

These were removed. (See individual commits.)

---

_@charliermarsh reviewed on 2024-04-03 23:15_

---

_Review comment by @charliermarsh on `crates/uv-git/src/git.rs`:33 on 2024-04-03 23:15_

This was renamed from `Ref`.

---

_@charliermarsh reviewed on 2024-04-03 23:15_

---

_Review comment by @charliermarsh on `crates/uv-git/src/git.rs`:39 on 2024-04-03 23:15_

This is now `BranchOrTagOrCommit`.

---

_Comment by @zanieb on 2024-04-03 23:55_

I'll review this tonight.

---

_@BurntSushi approved on 2024-04-04 00:39_

Looks reasonable to me. I liked the commit breakdown. :)

---

_@zanieb approved on 2024-04-04 01:40_

---

_Merged by @charliermarsh on 2024-04-04 02:05_

---

_Closed by @charliermarsh on 2024-04-04 02:05_

---

_Branch deleted on 2024-04-04 02:05_

---
