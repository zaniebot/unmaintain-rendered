```yaml
number: 46
title: Build on every push but only publish on tags
type: pull_request
state: merged
author: adriangb
labels: []
assignees: []
merged: true
base: main
head: patch-1
created_at: 2022-08-30T14:02:12Z
updated_at: 2022-08-30T17:20:04Z
url: https://github.com/astral-sh/ruff/pull/46
synced_at: 2026-01-12T06:00:50Z
```

# Build on every push but only publish on tags

---

_Pull request opened by @adriangb on 2022-08-30 14:02_

_No description provided._

---

_@charliermarsh reviewed on 2022-08-30 15:54_

---

_Review comment by @charliermarsh on `.github/workflows/release.yaml`:270 on 2022-08-30 15:54_

Can this condition be true? If `github.ref` is exactly `'refs/heads/main'`, then how could it start with `'refs/tags/'`?

---

_@adriangb reviewed on 2022-08-30 16:20_

---

_Review comment by @adriangb on `.github/workflows/release.yaml`:270 on 2022-08-30 16:20_

Good catch. I guess we can just remove the `refs/heads/main` part as long as you're the only one creating tags.

---

_@adriangb reviewed on 2022-08-30 16:41_

---

_Review comment by @adriangb on `.github/workflows/release.yaml`:270 on 2022-08-30 16:41_

Done!

---

_Comment by @adriangb on 2022-08-30 16:41_

Now all this is doing is running the workflow on every push (which generates the artifacts as a GitHub artifact tied to the commit) but doesn't publish them on PyPi unless there's a tag. Also fixing a leftover from `lint`. I think you'll be able to get rid of the `maturin_build` and maybe `cargo built` jobs in `ci.yaml` right?

---

_@charliermarsh approved on 2022-08-30 17:19_

Thanks!

---

_Merged by @charliermarsh on 2022-08-30 17:19_

---

_Closed by @charliermarsh on 2022-08-30 17:19_

---

_Branch deleted on 2022-08-30 17:20_

---
