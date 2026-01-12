```yaml
number: 867
title: "Fix clippy::default-trait-access (pedantic)"
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: clippy-default-trait-access
created_at: 2022-11-22T01:36:31Z
updated_at: 2022-11-22T02:00:38Z
url: https://github.com/astral-sh/ruff/pull/867
synced_at: 2026-01-12T15:55:05Z
```

# Fix clippy::default-trait-access (pedantic)

---

_@andersk_

Specific `T::default()` calls are more readable than `Default::default()`.

https://rust-lang.github.io/rust-clippy/master/index.html#default_trait_access

---

_Renamed from "Fix clippy::default-trait-access" to "Fix clippy::default-trait-access (pedantic)" by @andersk on 2022-11-22 01:39_

---

_Merged by @charliermarsh on 2022-11-22 02:00_

---

_Closed by @charliermarsh on 2022-11-22 02:00_

---
