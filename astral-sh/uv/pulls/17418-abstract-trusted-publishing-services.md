```yaml
number: 17418
title: Abstract Trusted Publishing services
type: pull_request
state: merged
author: woodruffw
labels:
  - internal
assignees: []
merged: true
base: main
head: ww/pyx-tp
created_at: 2026-01-12T15:50:36Z
updated_at: 2026-01-13T15:53:59Z
url: https://github.com/astral-sh/uv/pull/17418
synced_at: 2026-01-13T16:27:49Z
```

# Abstract Trusted Publishing services

---

_@woodruffw_

## Summary

This is the first of two PRs to enable direct Trusted Publishing to pyx, without an intermediating step (like `pyx-auth-action`).

This first PR contains no functional changes, just a refactor to make the actual functional changes clearer. Specifically, it adds a `TrustedPublishingService` trait that describes the behavior of a service that is Trusted Publishing aware, and adds an initial `PyPIPublishingService` implementation of that trait that matches the current PyPI Trusted Publishing APIs. I'll use the same trait shape to define a `PyxPublishingService` in a subsequent PR.

(Longer term, once PEP 807 is implemented, we'll also be able to add a `StandardPublishingService` or whatever and deprecate the other two.)

## Test Plan

No functional changes. Should be covered by the existing publishing tests.

---

_Review requested from @konstin by @woodruffw on 2026-01-12 15:50_

---

_Assigned to @woodruffw by @woodruffw on 2026-01-12 15:50_

---

_Label `internal` added by @woodruffw on 2026-01-12 15:50_

---

_Marked ready for review by @woodruffw on 2026-01-12 15:54_

---

_@zanieb reviewed on 2026-01-12 23:57_

---

_Review comment by @zanieb on `crates/uv-publish/src/trusted_publishing/pypi.rs`:3 on 2026-01-12 23:57_

It's a little funny to use backticks for Test PyPI? Want to just add that to the known names?

---

_@zanieb reviewed on 2026-01-12 23:57_

---

_Review comment by @zanieb on `crates/uv-publish/src/trusted_publishing/pypi.rs`:3 on 2026-01-12 23:57_

(I probably wouldn't mention it all tbh)

---

_@zanieb approved on 2026-01-12 23:57_

---

_@woodruffw reviewed on 2026-01-13 00:14_

---

_Review comment by @woodruffw on `crates/uv-publish/src/trusted_publishing/pypi.rs`:3 on 2026-01-13 00:14_

Yeah, will remove. I don't remember even putting backticks there 🤔

---

_Merged by @woodruffw on 2026-01-13 15:53_

---

_Closed by @woodruffw on 2026-01-13 15:53_

---

_Branch deleted on 2026-01-13 15:53_

---
