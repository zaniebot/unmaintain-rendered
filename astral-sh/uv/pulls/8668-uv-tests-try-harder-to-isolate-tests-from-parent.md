```yaml
number: 8668
title: "uv/tests: try harder to isolate tests from parent git repositories"
type: pull_request
state: merged
author: BurntSushi
labels:
  - testing
assignees: []
merged: true
base: main
head: ag/fix-git-author-info
created_at: 2024-10-29T16:41:54Z
updated_at: 2024-10-29T17:16:05Z
url: https://github.com/astral-sh/uv/pull/8668
synced_at: 2026-01-10T12:54:14Z
```

# uv/tests: try harder to isolate tests from parent git repositories

---

_Pull request opened by @BurntSushi on 2024-10-29 16:41_

Previously, when tests were run in `~/.local/share/uv`, the behavior of
some tests could be impacted by a git repository in `~` (as on my
system). To avoid this, we set `GIT_CEILING_DIRECTORIES` to forcefully
prevent git from climbing out of its test directory to look for parent
git repositories.


---

_Review requested from @zanieb by @BurntSushi on 2024-10-29 16:42_

---

_@zanieb approved on 2024-10-29 16:44_

We could apply this to all test commands, I think.

---

_Label `testing` added by @zanieb on 2024-10-29 16:44_

---

_Comment by @BurntSushi on 2024-10-29 16:58_

@zanieb Done deal.

---

_Merged by @BurntSushi on 2024-10-29 17:16_

---

_Closed by @BurntSushi on 2024-10-29 17:16_

---

_Branch deleted on 2024-10-29 17:16_

---
