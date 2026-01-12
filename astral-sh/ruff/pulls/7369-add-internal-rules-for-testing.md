```yaml
number: 7369
title: Add internal rules for testing
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: zanie/rule-tests
created_at: 2023-09-13T21:00:52Z
updated_at: 2024-01-31T19:12:40Z
url: https://github.com/astral-sh/ruff/pull/7369
synced_at: 2026-01-12T15:55:23Z
```

# Add internal rules for testing

---

_@zanieb_

Follow-up to discussion in #7210

Moves integration tests from using rules that are transitively in nursery / preview groups to dedicated test rules that only exist during development. These rules _always_ raise violations (they do not require specific file behavior).

Uses features instead of `cfg(test)` for cross-crate support per https://github.com/rust-lang/cargo/issues/8379

---

_Comment by @zanieb on 2023-09-13 22:22_

Need to filter the rules from the schema, probably by editing the proc macro.

---

_@MichaReiser approved on 2023-09-15 17:02_

Looks okay to me

---

_@konstin approved on 2023-09-18 08:13_

---

_Comment by @zanieb on 2024-01-31 19:12_

Replaced by #9747

---

_Closed by @zanieb on 2024-01-31 19:12_

---
