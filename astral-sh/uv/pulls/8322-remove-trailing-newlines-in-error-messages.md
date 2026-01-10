```yaml
number: 8322
title: Remove trailing newlines in error messages
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/remove-trailing-newlines
created_at: 2024-10-18T09:45:18Z
updated_at: 2024-10-18T15:17:43Z
url: https://github.com/astral-sh/uv/pull/8322
synced_at: 2026-01-10T12:54:07Z
```

# Remove trailing newlines in error messages

---

_Pull request opened by @konstin on 2024-10-18 09:45_

Sometimes, multiline errors bring their own trailing newlines, which double with our trailing newlines. We need to trim those.

Before:

![image](https://github.com/user-attachments/assets/3ae41dcf-475d-4b9d-8833-2de3ea1c4911)

After:

![image](https://github.com/user-attachments/assets/e7497990-053b-4b91-8ab5-a893d4e8a7fd)


---

_Label `enhancement` added by @konstin on 2024-10-18 09:45_

---

_Review requested from @charliermarsh by @konstin on 2024-10-18 09:45_

---

_@charliermarsh approved on 2024-10-18 15:02_

Slightly dangerous maybe... but seems ok? I can't tell hah.

---

_Merged by @konstin on 2024-10-18 15:17_

---

_Closed by @konstin on 2024-10-18 15:17_

---

_Branch deleted on 2024-10-18 15:17_

---
