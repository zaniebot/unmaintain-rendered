```yaml
number: 3725
title: Track editable requirements in lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/ed
created_at: 2024-05-21T22:40:37Z
updated_at: 2024-05-22T13:06:19Z
url: https://github.com/astral-sh/uv/pull/3725
synced_at: 2026-01-10T14:32:20Z
```

# Track editable requirements in lockfile

---

_Pull request opened by @charliermarsh on 2024-05-21 22:40_

## Summary

This PR adds editables using a new source type (`editable+...`), and then extracts the editables from the lockfile in `uv sync`.

Closes https://github.com/astral-sh/uv/issues/3695.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-21 22:40_

---

_Review requested from @konstin by @charliermarsh on 2024-05-21 22:40_

---

_Label `preview` added by @charliermarsh on 2024-05-21 22:40_

---

_@charliermarsh reviewed on 2024-05-21 22:41_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:659 on 2024-05-21 22:41_

Alternatively, we can reuse `SourceKind::Directory` and encode the editable somewhere else... like on the URL?

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/sync.rs`:116 on 2024-05-21 22:41_

We really need to improve the situation around editables, this is not good. But it's out of scope for this PR.

---

_@charliermarsh reviewed on 2024-05-21 22:42_

---

_Comment by @konstin on 2024-05-22 09:31_

One thing i'm thinking about: If the user switches a dep between editable and regular, does this change the resolution, would this be a change in the lockfile? Assuming regular and editable builds have the same metadata, we could make this an install time switch.

---

_@konstin approved on 2024-05-22 09:31_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:659 on 2024-05-22 11:56_

I think a new source kind makes sense to me. Not 100% sure though.

---

_@BurntSushi approved on 2024-05-22 11:57_

LGTM

---

_Comment by @BurntSushi on 2024-05-22 11:58_

> One thing i'm thinking about: If the user switches a dep between editable and regular, does this change the resolution, would this be a change in the lockfile? Assuming regular and editable builds have the same metadata, we could make this an install time switch.

My inclination is to say that this changes the lock file because it will change the "source" of the dependency. Or at least, I think that is the more conservative option that perhaps we ought to start with. Maybe practice will reveal that we want more flexibility though.

---

_Merged by @charliermarsh on 2024-05-22 13:06_

---

_Closed by @charliermarsh on 2024-05-22 13:06_

---

_Branch deleted on 2024-05-22 13:06_

---
