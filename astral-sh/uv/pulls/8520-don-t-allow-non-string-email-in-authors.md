```yaml
number: 8520
title: "Don't allow non-string email in authors"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: konsti/contact-string
created_at: 2024-10-24T09:18:51Z
updated_at: 2024-10-24T15:25:53Z
url: https://github.com/astral-sh/uv/pull/8520
synced_at: 2026-01-10T12:54:11Z
```

# Don't allow non-string email in authors

---

_Pull request opened by @konstin on 2024-10-24 09:18_

Previously, the case below would pass by matching the name variant, ignoring the invalid email address.

```toml
[project]
name = "hello-world"
version = "0.1.0"
authors = [
    { name = "John Doe", email = 1 }
]
```

Usually, we don't use `deny_unknown_fields` for blocking too much, but here it's needed to prevent fallthrough (it avoids writing a custom deserializer).

This affects the build backend (preview) only.

---

_Label `enhancement` added by @konstin on 2024-10-24 09:18_

---

_Label `preview` added by @konstin on 2024-10-24 09:18_

---

_Review requested from @BurntSushi by @konstin on 2024-10-24 09:18_

---

_@zanieb approved on 2024-10-24 12:40_

---

_Merged by @konstin on 2024-10-24 15:25_

---

_Closed by @konstin on 2024-10-24 15:25_

---

_Branch deleted on 2024-10-24 15:25_

---
