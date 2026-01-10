```yaml
number: 438
title: Switch from vendored PubGrub to a fork
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/pubgrub-fork
created_at: 2023-11-16T18:58:04Z
updated_at: 2023-11-16T19:49:20Z
url: https://github.com/astral-sh/uv/pull/438
synced_at: 2026-01-10T15:50:28Z
```

# Switch from vendored PubGrub to a fork

---

_Pull request opened by @zanieb on 2023-11-16 18:58_

A fork will let us stay up to date with the upstream while replaying our work on top of it.

I expect a similar workflow to the RustPython-Parser fork we maintained, except that I wrote an automation to create tags for each commit on the fork (https://github.com/zanieb/pubgrub/pull/2) so we do not need to manually tag and document each commit.

To update with the upstream:

- Rebase our fork's `main` branch on top of the latest changes in upstream's `dev` branch
- Force push, overwriting our `main` branch history
- Change the commit hash here to the last commit on `main` in our fork

Since we automatically tag each commit on the fork, we should never lose the commits that are dropped from `main` during rebase.


---

_@charliermarsh reviewed on 2023-11-16 18:59_

---

_Review comment by @charliermarsh on `Cargo.toml`:50 on 2023-11-16 18:59_

I think we should probably tag this branch, and just add the tag as a comment, like we did for the parser. Though I'm extremely open to better ideas...

---

_@charliermarsh approved on 2023-11-16 18:59_

---

_@zanieb reviewed on 2023-11-16 19:31_

---

_Review comment by @zanieb on `Cargo.toml`:50 on 2023-11-16 19:31_

Behold my suggestion :) https://github.com/zanieb/pubgrub/pull/2

I think this will work well without the overhead?

---

_Marked ready for review by @zanieb on 2023-11-16 19:32_

---

_@zanieb reviewed on 2023-11-16 19:41_

---

_Review comment by @zanieb on `Cargo.toml`:50 on 2023-11-16 19:41_

I see no reason to comment with the tag since it should be trivial to find the tag for any commit?

---

_Merged by @zanieb on 2023-11-16 19:49_

---

_Closed by @zanieb on 2023-11-16 19:49_

---

_Branch deleted on 2023-11-16 19:49_

---
