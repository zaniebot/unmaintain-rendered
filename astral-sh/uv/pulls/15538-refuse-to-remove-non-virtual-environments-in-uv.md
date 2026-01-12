```yaml
number: 15538
title: "Refuse to remove non-virtual environments in `uv venv`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/clear
created_at: 2025-08-26T16:35:37Z
updated_at: 2025-08-26T17:26:23Z
url: https://github.com/astral-sh/uv/pull/15538
synced_at: 2026-01-12T16:11:48Z
```

# Refuse to remove non-virtual environments in `uv venv`

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/15474.


---

_Review requested from @zanieb by @charliermarsh on 2025-08-26 16:43_

---

_Label `bug` added by @charliermarsh on 2025-08-26 16:43_

---

_Marked ready for review by @charliermarsh on 2025-08-26 16:44_

---

_Comment by @charliermarsh on 2025-08-26 16:44_

I'm a little wary of doing this for non-interactive terminals since it's breaking... I wonder if we should just warn in that case like we do today.

---

_Comment by @notatallshaw on 2025-08-26 16:49_

> I'm a little wary of doing this for non-interactive terminals since it's breaking... I wonder if we should just warn in that case like we do today.

As long as I can get the behavior where it throws an error, see also https://github.com/astral-sh/uv/issues/15475

As someone who writes scripts having commands behave differently when I run them locally vs CI is maddening and makes it really difficult to debug what went wrong.



---

_Comment by @zanieb on 2025-08-26 16:50_

For what it's worth, this is a pretty clear regression from #14309. I don't think we should be deleting arbitrary directories without opt-in.

I think the only questionable part is when `--clear` is explicitly provided. I would be okay deferring that to a future release.

---

_Comment by @zanieb on 2025-08-26 16:51_

> As long as I can get the behavior where it throws an error, see also https://github.com/astral-sh/uv/issues/15475

I think we should definitely not prompt for non-virtual environments.

---

_Comment by @zanieb on 2025-08-26 16:52_

(I don't think you need to keep saying that you don't like the TTY behavior, I think your point is clear there)

---

_Comment by @charliermarsh on 2025-08-26 16:53_

Ah I see, yeah it is a regression in that case. Okay, let's ship this for non-`--clear`, and leave `--clear` as-is?

---

_Comment by @notatallshaw on 2025-08-26 16:53_

> (I don't think you need to keep saying that you don't like the TTY behavior, I think your point is clear there)

Noted, it wasn't clear to me @charliermarsh was aware of that discussion. 

---

_@zanieb reviewed on 2025-08-26 16:54_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:127 on 2025-08-26 16:54_

This is the part I would consider moving into a breaking release / requiring a `--force` flag. 

---

_@zanieb reviewed on 2025-08-26 16:54_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:143 on 2025-08-26 16:54_

I'm okay with this being breaking since it was a 0.8 regression and I consider it a significant bug.

---

_@zanieb approved on 2025-08-26 17:00_

👍 presuming we defer the breaking change in `OnExisting::Remove`

---

_Merged by @charliermarsh on 2025-08-26 17:26_

---

_Closed by @charliermarsh on 2025-08-26 17:26_

---

_Branch deleted on 2025-08-26 17:26_

---
