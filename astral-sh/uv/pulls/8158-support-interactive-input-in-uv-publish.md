```yaml
number: 8158
title: "Support interactive input in `uv publish`"
type: pull_request
state: merged
author: blueraft
labels:
  - preview
assignees: []
merged: true
base: main
head: interactive-publish
created_at: 2024-10-13T15:55:13Z
updated_at: 2024-10-15T11:23:49Z
url: https://github.com/astral-sh/uv/pull/8158
synced_at: 2026-01-10T12:54:04Z
```

# Support interactive input in `uv publish`

---

_Pull request opened by @blueraft on 2024-10-13 15:55_

## Summary

Resolves https://github.com/astral-sh/uv/issues/8073

## Test Plan

<img width="733" alt="Screenshot 2024-10-13 at 17 53 47" src="https://github.com/user-attachments/assets/1acc6e67-027b-4dcd-adbb-566e0dc4839e">

Not sure how to test in CI though 🤔

---

_Review requested from @konstin by @charliermarsh on 2024-10-14 14:47_

---

_Review comment by @konstin on `crates/uv/src/commands/publish.rs`:122 on 2024-10-14 20:29_

We should not error in this case, but return `(None, None)` and later in the publish code error if we don't have any credentials. uv often runs in non-interactive environments such as CI.

---

_Review comment by @konstin on `crates/uv-console/src/lib.rs`:101 on 2024-10-14 20:34_

Currently, the username allows an empty value, while the password field doesn't, we should use one behavior for both

---

_Label `preview` added by @konstin on 2024-10-14 20:40_

---

_Comment by @konstin on 2024-10-14 20:40_

> Not sure how to test in CI though 🤔

I'll check if i can find a good method, but we can also merge without CI test coverage.

The CI failures are unrelated btw.

---

_@konstin approved on 2024-10-14 20:40_

---

_@blueraft reviewed on 2024-10-14 21:07_

---

_Review comment by @blueraft on `crates/uv/src/commands/publish.rs`:122 on 2024-10-14 21:07_

Done

---

_@blueraft reviewed on 2024-10-14 21:09_

---

_Review comment by @blueraft on `crates/uv-console/src/lib.rs`:101 on 2024-10-14 21:09_

I've gone with allowing empty values in both, since that's a bit more flexible. Let me know if you prefer it the other way

---

_@konstin reviewed on 2024-10-15 08:00_

---

_Review comment by @konstin on `crates/uv-console/src/lib.rs`:101 on 2024-10-15 08:00_

:+1:


---

_Merged by @konstin on 2024-10-15 08:00_

---

_Closed by @konstin on 2024-10-15 08:00_

---

_Comment by @konstin on 2024-10-15 08:00_

Thanks!

---

_Branch deleted on 2024-10-15 11:23_

---
