```yaml
number: 8827
title: "Publish: Warn when keyring has no password"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: konsti/check-keyring
created_at: 2024-11-05T08:44:53Z
updated_at: 2024-11-27T19:54:50Z
url: https://github.com/astral-sh/uv/pull/8827
synced_at: 2026-01-10T12:00:00Z
```

# Publish: Warn when keyring has no password

---

_Pull request opened by @konstin on 2024-11-05 08:44_

When trying to upload without a password but with the keyring, check that the keyring has a password for the upload URL and username and warn if it doesn't.

Fixes #8781

---

_Label `enhancement` added by @konstin on 2024-11-05 08:44_

---

_Label `preview` added by @konstin on 2024-11-05 08:44_

---

_@zanieb reviewed on 2024-11-27 16:46_

---

_Review comment by @zanieb on `crates/uv/tests/it/publish.rs`:212 on 2024-11-27 16:46_

I want to avoid unbounded dependencies in the test suite for security. Can we continuously test against the latest keyring in a dedicated CI job instead so it doesn't run on developer machines?

---

_Review comment by @zanieb on `crates/uv/tests/it/publish.rs`:220 on 2024-11-27 16:46_

Did you know we have a dedicated test keyring plugin? Might make life easier

https://github.com/astral-sh/uv/blob/cc6bfa14d169b5da2aca0aa0b435343138f7fec9/crates/uv/tests/it/lock.rs#L16221-L16272

---

_@zanieb reviewed on 2024-11-27 16:46_

---

_@konstin reviewed on 2024-11-27 19:23_

---

_Review comment by @konstin on `crates/uv/tests/it/publish.rs`:220 on 2024-11-27 19:23_

That's neat!

---

_@zanieb approved on 2024-11-27 19:27_

---

_Merged by @konstin on 2024-11-27 19:54_

---

_Closed by @konstin on 2024-11-27 19:54_

---

_Branch deleted on 2024-11-27 19:54_

---
