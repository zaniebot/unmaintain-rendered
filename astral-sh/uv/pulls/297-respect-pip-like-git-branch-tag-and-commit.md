```yaml
number: 297
title: Respect pip-like Git branch, tag, and commit references
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/hash
created_at: 2023-11-02T18:45:49Z
updated_at: 2023-11-02T19:10:04Z
url: https://github.com/astral-sh/uv/pull/297
synced_at: 2026-01-12T16:03:51Z
```

# Respect pip-like Git branch, tag, and commit references

---

_@charliermarsh_

We need to parse revisions out from URLs like `MyProject @ git+https://git.example.com/MyProject.git@v1.0`, per [VCS Support](https://pip.pypa.io/en/stable/topics/vcs-support/). Cargo has the advantage that it uses a TOML table in its configuration, so the user has to specify whether they're fetching a commit, a tag, a branch, etc. We have to instead assume that anything that isn't clearly a commit is _either_ a branch or a tag.

Closes https://github.com/astral-sh/puffin/issues/296.

---

_Marked ready for review by @charliermarsh on 2023-11-02 18:46_

---

_Review requested from @zanieb by @charliermarsh on 2023-11-02 18:46_

---

_@charliermarsh reviewed on 2023-11-02 18:47_

---

_Review comment by @charliermarsh on `crates/puffin-git/src/git.rs`:26 on 2023-11-02 18:47_

I moved this into `git.rs` from `mod.rs` since it now relies on some internals of this module. It makes sense here anyway.

---

_@charliermarsh reviewed on 2023-11-02 18:47_

---

_Review comment by @charliermarsh on `crates/puffin-git/src/git.rs`:913 on 2023-11-02 18:47_

Possibly a better way to do this? Not sure, this was easiest and works.

---

_@charliermarsh reviewed on 2023-11-02 18:47_

---

_Review comment by @charliermarsh on `crates/puffin-git/src/lib.rs`:42 on 2023-11-02 18:47_

Returning to this in a future PR -- it's unused as-is anyway.

---

_@charliermarsh reviewed on 2023-11-02 18:50_

---

_Review comment by @charliermarsh on `crates/puffin-git/src/lib.rs`:34 on 2023-11-02 18:50_

I verified that this is what pip does.

---

_@zanieb approved on 2023-11-02 18:54_

Sweet

---

_@zanieb reviewed on 2023-11-02 18:55_

---

_Review comment by @zanieb on `crates/puffin-git/src/git.rs`:241 on 2023-11-02 18:55_

Just wondering, why this order?

---

_@charliermarsh reviewed on 2023-11-02 19:02_

---

_Review comment by @charliermarsh on `crates/puffin-git/src/git.rs`:241 on 2023-11-02 19:02_

Totally arbitrary, do you have a preference? (I'm genuinely unsure.)

---

_Review comment by @zanieb on `crates/puffin-git/src/git.rs`:241 on 2023-11-02 19:06_

Well... I guess `git checkout` does a branch

```
❯ git tag v0.1.0
❯ gcm
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
❯ gcb v0.1.0
Switched to a new branch 'v0.1.0'
❯ gcm
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
❯ gco v0.1.0
warning: refname 'v0.1.0' is ambiguous.
Switched to branch 'v0.1.0'
```

but for _installing_ packages I would much rather match a tag than a branch if there's a collision.

---

_@zanieb reviewed on 2023-11-02 19:06_

---

_@charliermarsh reviewed on 2023-11-02 19:09_

---

_Review comment by @charliermarsh on `crates/puffin-git/src/git.rs`:241 on 2023-11-02 19:09_

Hmm... I kind of assume pip does branch-then-tag then. I'll just leave it for now, we can revisit. (Thanks for checking that, it's good to know.)

---

_Merged by @charliermarsh on 2023-11-02 19:10_

---

_Closed by @charliermarsh on 2023-11-02 19:10_

---

_Branch deleted on 2023-11-02 19:10_

---
