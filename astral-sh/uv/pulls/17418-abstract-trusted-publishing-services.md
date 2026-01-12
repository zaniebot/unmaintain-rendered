```yaml
number: 17418
title: Abstract Trusted Publishing services
type: pull_request
state: open
author: woodruffw
labels:
  - internal
assignees: []
base: main
head: ww/pyx-tp
created_at: 2026-01-12T15:50:36Z
updated_at: 2026-01-12T15:54:41Z
url: https://github.com/astral-sh/uv/pull/17418
synced_at: 2026-01-12T16:14:07Z
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
