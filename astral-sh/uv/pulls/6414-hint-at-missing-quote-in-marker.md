```yaml
number: 6414
title: Hint at missing quote in marker
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/better-missing-quote-string
created_at: 2024-08-22T08:09:21Z
updated_at: 2024-08-23T14:27:27Z
url: https://github.com/astral-sh/uv/pull/6414
synced_at: 2026-01-12T16:07:21Z
```

# Hint at missing quote in marker

---

_@konstin_

For https://github.com/astral-sh/uv/issues/6379#issuecomment-2303074836.

Input: `name; python_version == 3.10`
Error:
```
Expected a quoted string or a valid marker name, found '3.10'
name; python_version == 3.10
                        ^^^^
```

---

_Label `enhancement` added by @konstin on 2024-08-22 08:09_

---

_Merged by @konstin on 2024-08-22 08:15_

---

_Closed by @konstin on 2024-08-22 08:15_

---

_Branch deleted on 2024-08-22 08:15_

---

_Label `enhancement` removed by @konstin on 2024-08-22 08:24_

---

_Label `error messages` added by @konstin on 2024-08-22 08:24_

---
