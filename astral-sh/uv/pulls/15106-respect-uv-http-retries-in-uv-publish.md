```yaml
number: 15106
title: "Respect `UV_HTTP_RETRIES` in `uv publish`"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/respect-retries-in-upload
created_at: 2025-08-06T11:53:38Z
updated_at: 2025-08-06T15:59:19Z
url: https://github.com/astral-sh/uv/pull/15106
synced_at: 2026-01-12T16:11:35Z
```

# Respect `UV_HTTP_RETRIES` in `uv publish`

---

_@konstin_

Previously, publish would always use the default retries, now it respects `UV_HTTP_RETRIES`

Some awkward error handling to avoid pulling anyhow into uv-publish.

---

_Label `bug` added by @konstin on 2025-08-06 11:53_

---

_Review comment by @jtfmumm on `crates/uv-client/src/base_client.rs`:46 on 2025-08-06 13:27_

Should we put this behind a `#[cfg(test)]`?


---

_@jtfmumm reviewed on 2025-08-06 13:28_

---

_@jtfmumm approved on 2025-08-06 13:31_

---

_@konstin reviewed on 2025-08-06 15:59_

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:46 on 2025-08-06 15:59_

We can't unfortunately as `#[cfg(test)]` is crate-local.

---

_Merged by @konstin on 2025-08-06 15:59_

---

_Closed by @konstin on 2025-08-06 15:59_

---

_Branch deleted on 2025-08-06 15:59_

---
