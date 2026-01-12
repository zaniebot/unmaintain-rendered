```yaml
number: 7864
title: "Preserve case-insensitive sorts in `uv add`"
type: pull_request
state: merged
author: blueraft
labels:
  - enhancement
assignees: []
merged: true
base: main
head: add-sort
created_at: 2024-10-02T09:49:13Z
updated_at: 2024-10-04T10:57:38Z
url: https://github.com/astral-sh/uv/pull/7864
synced_at: 2026-01-12T16:08:03Z
```

# Preserve case-insensitive sorts in `uv add`

---

_@blueraft_

## Summary

Resolves https://github.com/astral-sh/uv/issues/7801

## Test Plan

`cargo test`

---

_@charliermarsh reviewed on 2024-10-02 11:18_

---

_Review comment by @charliermarsh on `crates/uv/tests/edit.rs`:4695 on 2024-10-02 11:18_

I think we might want to preserve _either_ case-sensitive or case-insensitive sorts, rather than always using a case-insensitive sort. What do you think?

---

_@blueraft reviewed on 2024-10-02 11:44_

---

_Review comment by @blueraft on `crates/uv/tests/edit.rs`:4695 on 2024-10-02 11:44_

I see what you mean, I'd probably prefer case insensitive sort as that's probably the most common case. In this example, I'd expect `anyio` to come before `CacheControl`. But happy to do it either way.

---

_@charliermarsh reviewed on 2024-10-03 13:22_

---

_Review comment by @charliermarsh on `crates/uv/tests/edit.rs`:4695 on 2024-10-03 13:22_

I'm happy to default to case insensitive, but if the user has intentionally sorted as case sensitive, I think it makes sense to preserve it?

---

_@blueraft reviewed on 2024-10-03 16:58_

---

_Review comment by @blueraft on `crates/uv/tests/edit.rs`:4695 on 2024-10-03 16:58_

Done, I've also added a test for the case-sensitive case.

---

_@charliermarsh approved on 2024-10-04 10:51_

Great, thanks!

---

_Label `enhancement` added by @charliermarsh on 2024-10-04 10:51_

---

_Merged by @charliermarsh on 2024-10-04 10:57_

---

_Closed by @charliermarsh on 2024-10-04 10:57_

---
