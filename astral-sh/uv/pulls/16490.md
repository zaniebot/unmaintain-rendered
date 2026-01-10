```yaml
number: 16490
title: "Add an empty group with `uv add -r`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/empty
created_at: 2025-10-29T00:38:08Z
updated_at: 2025-10-29T15:31:05Z
url: https://github.com/astral-sh/uv/pull/16490
synced_at: 2026-01-10T06:36:16Z
```

# Add an empty group with `uv add -r`

---

_Pull request opened by @charliermarsh on 2025-10-29 00:38_

## Summary

`uv add -r requirements.txt --group foo` will now create `foo` even if `requirements.txt` is empty.

Closes https://github.com/astral-sh/uv/issues/16361.


---

_Review requested from @zanieb by @charliermarsh on 2025-10-29 00:38_

---

_Review requested from @konstin by @charliermarsh on 2025-10-29 00:38_

---

_Label `bug` added by @charliermarsh on 2025-10-29 00:38_

---

_Marked ready for review by @charliermarsh on 2025-10-29 00:38_

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject_mut.rs`:741 on 2025-10-29 11:00_

Why are we doing this check for dependency groups, but not for extras?

---

_@konstin reviewed on 2025-10-29 11:02_

---

_Comment by @zanieb on 2025-10-29 14:02_

I think I'd expect this to fail? but I'm okay with this behavior too

---

_@charliermarsh reviewed on 2025-10-29 15:30_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/pyproject_mut.rs`:741 on 2025-10-29 15:30_

I'm not sure but it's consistent with the other methods. I can look in a separate PR.

---

_@konstin approved on 2025-10-29 15:30_

---

_Comment by @charliermarsh on 2025-10-29 15:30_

Yeah, I was unsure, but I think it's nice for it not to fail if you're invoking it programmatically because there could be legitimate cases where you might end up with an empty `requirements.txt`. (That kind of thing happens in pyx sometimes when we build Lambdas.)

---

_Merged by @charliermarsh on 2025-10-29 15:31_

---

_Closed by @charliermarsh on 2025-10-29 15:31_

---

_Branch deleted on 2025-10-29 15:31_

---
