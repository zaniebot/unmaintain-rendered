```yaml
number: 2601
title: Use dense formatting for requirement version specifiers in diagnostics
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/format-requirement
created_at: 2024-03-21T23:06:27Z
updated_at: 2024-03-22T21:35:50Z
url: https://github.com/astral-sh/uv/pull/2601
synced_at: 2026-01-12T16:05:07Z
```

# Use dense formatting for requirement version specifiers in diagnostics

---

_@zanieb_

For consistency with the output in "no solution" errors


---

_Label `cli` added by @zanieb on 2024-03-21 23:06_

---

_Review comment by @konstin on `crates/uv/tests/pip_check.rs`:210 on 2024-03-22 09:05_

I find this one hard to read, should split name and specifier?

---

_@konstin approved on 2024-03-22 09:05_

---

_@zanieb reviewed on 2024-03-22 14:19_

---

_Review comment by @zanieb on `crates/uv/tests/pip_check.rs`:210 on 2024-03-22 14:19_

The reason we don't tend to split the name and specifier is that we don't use \` so it's less ambiguous as a single statement. I'm thinking we should probably remove the "`" here for consistency with our other messaging, but it's not really a priority. I just want us to have a consistent presentation.

Another option is we use colors to separate the specifier from the name i.e. bold names.

---

_@konstin reviewed on 2024-03-22 14:57_

---

_Review comment by @konstin on `crates/uv/tests/pip_check.rs`:210 on 2024-03-22 14:57_

i'd love using color for this, though still a space after the name for CI reports probably

---

_@zanieb reviewed on 2024-03-22 15:31_

---

_Review comment by @zanieb on `crates/uv/tests/pip_check.rs`:210 on 2024-03-22 15:31_

I'll look into that

---

_Merged by @zanieb on 2024-03-22 21:35_

---

_Closed by @zanieb on 2024-03-22 21:35_

---

_Branch deleted on 2024-03-22 21:35_

---
