```yaml
number: 5685
title: Generate CLI reference for documentation
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: zb/docs-reference
created_at: 2024-08-01T14:43:36Z
updated_at: 2024-08-01T16:04:18Z
url: https://github.com/astral-sh/uv/pull/5685
synced_at: 2026-01-12T16:06:57Z
```

# Generate CLI reference for documentation

---

_@zanieb_

Loosely based on [Cargo's format](https://github.com/rust-lang/cargo/blob/master/src/doc/src/commands/cargo-build.md)

<img width="896" alt="Screenshot 2024-08-01 at 9 44 03 AM" src="https://github.com/user-attachments/assets/7c016bb3-2b54-46af-8ea8-ce82e07a0e30">

Future work includes:

- Grouping options
- Enforcing some sort of specific command ordering
- Showing possible values for enums
- Adding "long_about" to commands for more context

---

_Label `documentation` added by @zanieb on 2024-08-01 14:43_

---

_Label `preview` added by @zanieb on 2024-08-01 14:43_

---

_Comment by @zanieb on 2024-08-01 14:45_

Notably the top-level preview commands are excluded because they're hidden

---

_Marked ready for review by @zanieb on 2024-08-01 15:05_

---

_Review requested from @charliermarsh by @zanieb on 2024-08-01 15:20_

---

_Review requested from @ibraheemdev by @zanieb on 2024-08-01 15:20_

---

_@zanieb reviewed on 2024-08-01 15:21_

---

_Review comment by @zanieb on `docs/stylesheets/extra.css`:151 on 2024-08-01 15:21_

I created a custom class, these have ~no reliance on mkdocs material

---

_@ibraheemdev approved on 2024-08-01 15:32_

Love this.

---

_Comment by @eth3lbert on 2024-08-01 15:38_

Great! I think I can help with future work part once we've finalized the grouping and ordering.

---

_Merged by @zanieb on 2024-08-01 16:04_

---

_Closed by @zanieb on 2024-08-01 16:04_

---

_Branch deleted on 2024-08-01 16:04_

---
