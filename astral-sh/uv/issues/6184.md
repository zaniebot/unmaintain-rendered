```yaml
number: 6184
title: Allow users to constrain universal locks to specific markers
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
created_at: 2024-08-18T18:22:00Z
updated_at: 2024-08-20T13:28:05Z
url: https://github.com/astral-sh/uv/issues/6184
synced_at: 2026-01-10T04:53:49Z
```

# Allow users to constrain universal locks to specific markers

---

_Issue opened by @charliermarsh on 2024-08-18 18:22_

I think we should let users apply a global marker constraint to "universal" resolution. E.g., maybe you don't care about locking on Windows, so you specify `platform_system != 'Windows'` in your `pyproject.toml`, and we avoid solving any branches that are Windows-only. Later, when installing, we'd error if the user is on an unsupported platform, based on those markers.

This would (1) make resolution a lot faster for users that only care about certain platforms, (2) enable us to solve locks that legitimately don't have solutions on certain platforms.

This would also solve https://github.com/astral-sh/uv/issues/4087.

---

_Label `enhancement` added by @charliermarsh on 2024-08-18 18:22_

---

_Label `preview` added by @charliermarsh on 2024-08-18 18:22_

---

_Comment by @zanieb on 2024-08-18 18:42_

I like it.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-18 22:06_

---

_Comment by @BurntSushi on 2024-08-19 14:03_

I like this a lot. I feel like it's a good release valve.

---

_Closed by @charliermarsh on 2024-08-20 13:28_

---

_Closed by @charliermarsh on 2024-08-20 13:28_

---
