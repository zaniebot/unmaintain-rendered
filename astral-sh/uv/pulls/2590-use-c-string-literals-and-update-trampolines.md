```yaml
number: 2590
title: Use c-string literals and update trampolines
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/use-c-string-literals
created_at: 2024-03-21T15:18:40Z
updated_at: 2024-03-21T15:36:02Z
url: https://github.com/astral-sh/uv/pull/2590
synced_at: 2026-01-12T16:05:07Z
```

# Use c-string literals and update trampolines

---

_@konstin_

Rust 1.77 has stabilized c-string literals (`c"<string>"`): https://doc.rust-lang.org/nightly/edition-guide/rust-2021/c-string-literals.html. This PR replaces the usages of the custom c-string literal macro in the trampoline with the new syntax.

---

_Label `enhancement` added by @konstin on 2024-03-21 15:18_

---

_@zanieb approved on 2024-03-21 15:20_

Sweet

---

_Comment by @zanieb on 2024-03-21 15:20_

Is this external facing? Or `internal`?

---

_Comment by @konstin on 2024-03-21 15:24_

It's internal but i've updated the trampolines so i wanted to put it into the release notes

---

_Merged by @konstin on 2024-03-21 15:36_

---

_Closed by @konstin on 2024-03-21 15:36_

---

_Branch deleted on 2024-03-21 15:36_

---
