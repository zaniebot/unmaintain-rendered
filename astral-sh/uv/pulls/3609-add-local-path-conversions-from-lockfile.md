```yaml
number: 3609
title: Add local path conversions from lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/dir-lock
created_at: 2024-05-15T16:28:29Z
updated_at: 2024-05-15T16:46:46Z
url: https://github.com/astral-sh/uv/pull/3609
synced_at: 2026-01-12T16:05:44Z
```

# Add local path conversions from lockfile

---

_@charliermarsh_

## Summary

Just does the most basic thing to convert from `Distribution` back to the installable type.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-15 16:28_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-05-15 16:28_

---

_Label `preview` added by @charliermarsh on 2024-05-15 16:28_

---

_Marked ready for review by @charliermarsh on 2024-05-15 16:28_

---

_@charliermarsh reviewed on 2024-05-15 16:29_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:280 on 2024-05-15 16:29_

It's a bit strange that we require both `url` and `path`, but I think the intent is that we do this fallible conversion when constructing to the type so that we don't have to do it everywhere that we _need_ the path.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:280 on 2024-05-15 16:33_

Yeah that makes sense I think.

(I am fond of the idea of encapsulating the distribution types and defining constructors for them. That way, you can add derived fields or whatever else without much fuss. But that's a big refactor and I am not altogether offended by pure data structs. They have their advantages...)

---

_@BurntSushi approved on 2024-05-15 16:34_

LGTM.

Probably after resolver forking is done, I'll plan to take another pass at the lock file format and smooth things out. Hopefully we can limp along as-is for now though.

---

_Merged by @charliermarsh on 2024-05-15 16:46_

---

_Closed by @charliermarsh on 2024-05-15 16:46_

---

_Branch deleted on 2024-05-15 16:46_

---
