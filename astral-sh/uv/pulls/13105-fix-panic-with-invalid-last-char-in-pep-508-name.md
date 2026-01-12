```yaml
number: 13105
title: Fix panic with invalid last char in PEP 508 name
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/invalid-pep508-name-panic
created_at: 2025-04-25T11:24:54Z
updated_at: 2025-04-25T12:56:47Z
url: https://github.com/astral-sh/uv/pull/13105
synced_at: 2026-01-12T16:10:32Z
```

# Fix panic with invalid last char in PEP 508 name

---

_@konstin_

Fixes #13102

---

_Label `bug` added by @konstin on 2025-04-25 11:24_

---

_Review requested from @BurntSushi by @konstin on 2025-04-25 11:24_

---

_Comment by @konstin on 2025-04-25 11:28_

The alternative would be:

```rust
if cursor.peek().map_or(
    true,
    |(_, char)| !matches!(char, 'A'..='Z' | 'a'..='z' | '0'..='9' | '.' | '-' | '_'),
) && matches!(char, '.' | '-' | '_')
```

---

_Closed by @konstin on 2025-04-25 11:29_

---

_Reopened by @konstin on 2025-04-25 11:29_

---

_@BurntSushi approved on 2025-04-25 12:53_

---

_Merged by @konstin on 2025-04-25 12:56_

---

_Closed by @konstin on 2025-04-25 12:56_

---

_Branch deleted on 2025-04-25 12:56_

---
