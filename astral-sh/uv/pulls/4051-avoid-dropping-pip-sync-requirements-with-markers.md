```yaml
number: 4051
title: "Avoid dropping `pip sync` requirements with markers"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/m
created_at: 2024-06-05T15:56:23Z
updated_at: 2024-06-05T16:05:48Z
url: https://github.com/astral-sh/uv/pull/4051
synced_at: 2026-01-10T13:54:02Z
```

# Avoid dropping `pip sync` requirements with markers

---

_Pull request opened by @charliermarsh on 2024-06-05 15:56_

## Summary

Thankfully this is pretty rare since `pip sync` is usually run on `pip compile` output, and `pip compile` never outputs markers.

Closes https://github.com/astral-sh/uv/issues/4044


---

_Label `bug` added by @charliermarsh on 2024-06-05 15:56_

---

_Marked ready for review by @charliermarsh on 2024-06-05 15:56_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-06-05 15:56_

---

_@charliermarsh reviewed on 2024-06-05 15:57_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:972 on 2024-06-05 15:57_

Similar to the comment below, there may be a better way to solve this. I have a feeling we might want a separate PubGrubPackageInner marker type.

---

_@BurntSushi approved on 2024-06-05 15:57_

Ahhhh right, I see now. The fix makes sense to me. Thank you!

---

_@BurntSushi reviewed on 2024-06-05 15:58_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:972 on 2024-06-05 15:58_

I have that same feeling, especially after you added the `Extra` variant.

---

_@zanieb approved on 2024-06-05 15:59_

---

_Merged by @charliermarsh on 2024-06-05 16:05_

---

_Closed by @charliermarsh on 2024-06-05 16:05_

---

_Branch deleted on 2024-06-05 16:05_

---
