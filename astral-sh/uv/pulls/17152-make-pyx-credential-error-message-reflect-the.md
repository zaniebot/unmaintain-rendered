```yaml
number: 17152
title: Make pyx credential error message reflect the realm
type: pull_request
state: open
author: zsol
labels:
  - enhancement
  - error messages
assignees: []
base: main
head: zsol/jj-zmwropzqtzxm
created_at: 2025-12-16T19:24:42Z
updated_at: 2026-01-12T11:46:17Z
url: https://github.com/astral-sh/uv/pull/17152
synced_at: 2026-01-12T16:12:38Z
```

# Make pyx credential error message reflect the realm

---

_@zsol_

This is unlikely to affect people who aren't pyx devs.

## Test Plan

`PYX_API_URL=http://localhost:8000 uv pip install --default-index $PYX_API_URL/simple foo` yields

```
Run `PYX_API_URL='http://localhost:8000' uv auth login http://localhost:8000` to authenticate uv with pyx
```

But the error message against production pyx.dev remains the same.


---

_Comment by @charliermarsh on 2025-12-16 19:25_

Should we include `PYX_API_URL` in the error message?

---

_Marked ready for review by @zsol on 2026-01-12 10:15_

---

_Review requested from @charliermarsh by @zsol on 2026-01-12 10:15_

---

_Review requested from @zanieb by @zsol on 2026-01-12 10:15_

---

_Comment by @konstin on 2026-01-12 11:33_

The env var syntax doesn't work on Windows, can we use a CLI parameter instead?

---

_Label `enhancement` added by @konstin on 2026-01-12 11:34_

---

_Label `error messages` added by @konstin on 2026-01-12 11:34_

---

_Comment by @zsol on 2026-01-12 11:46_

> The env var syntax doesn't work on Windows, can we use a CLI parameter instead?

I don't think there's a CLI parameter for this unfortunately. If you'd like, I can add OS-specific error messages (with the understanding that the message will be less helpful than it could be on Windows+cygwin or git bash)

---
