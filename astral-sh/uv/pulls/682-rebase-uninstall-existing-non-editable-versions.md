```yaml
number: 682
title: "Rebase: Uninstall existing non-editable versions when installing editable requirements bug"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/charlie/migrate-to-editable
created_at: 2023-12-18T09:23:59Z
updated_at: 2023-12-18T14:48:33Z
url: https://github.com/astral-sh/uv/pull/682
synced_at: 2026-01-12T16:04:07Z
```

# Rebase: Uninstall existing non-editable versions when installing editable requirements bug

---

_@konstin_

Separate branch for rebasing #677 onto main because i don't trust the rebase enough to force push.

Closes #677.

---

If you install `black` from PyPI, then `-e ../black`, we need to uninstall the existing `black`. This sounds simple, but that in turn requires that we _know_ `-e ../black` maps to the package `black`, so that we can mark it for uninstallation in the install plan. This, in turn, means that we need to build editable dependencies prior to the install plan.

This is just a bunch of reorganization to fix that specific bug (installing multiple versions of `black` if you run through the above workflow): we now run through the list of editables upfront, mark those that are already installed, build those that aren't, and then ensure that `InstallPlan` correctly removes those that need to be removed, etc.

Closes #676.

---

_Merged by @konstin on 2023-12-18 09:28_

---

_Closed by @konstin on 2023-12-18 09:28_

---

_Branch deleted on 2023-12-18 09:28_

---

_Comment by @charliermarsh on 2023-12-18 14:34_

@konstin - All good but in the future I might recommend using a merge commit to avoid the force-push, so retain the PR but also the history.

---

_Comment by @konstin on 2023-12-18 14:48_

The problem is a bit more complex here: #677 has two commits, one from #682 and one of its own. Since #682 had changes the first commit was clashing with main. I used graphite locally (cherry-pick would have also done) to smartly rebase the old first commit out and only keep the second, so i didn't do any merge at all. I'm not sure how to emulate that with a merge commit.

---
