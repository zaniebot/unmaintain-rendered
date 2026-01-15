```yaml
number: 17438
title: Support Trusted Publishing with pyx
type: pull_request
state: open
author: woodruffw
labels: []
assignees: []
draft: true
base: main
head: ww/pyx-tp-svc
created_at: 2026-01-13T15:59:46Z
updated_at: 2026-01-15T22:47:48Z
url: https://github.com/astral-sh/uv/pull/17438
synced_at: 2026-01-15T23:03:14Z
```

# Support Trusted Publishing with pyx

---

_@woodruffw_

## Summary

WIP. This follows #17418 and adds a `PyxPublishingService` that speaks the pyx-specific APIs for Trusted Publishing.

TODOs:

- [ ] Needs publishing integration tests (see below).
- [ ] Needs documentation.
- [x] Needs changes to `OidcTokenClaims` (these are currently GitHub-specific, they need to become an enum over various supported platforms).

## Test Plan

I'll add new publishing integration tests for the following scenarios:

- [x] Trusted Publishing between GitLab CI/CD <-> PyPI: #17443 
- [x] Trusted Publishing between GitHub Actions <-> pyx
- [ ] Trusted Publishing between GitLab CI/CD <-> pyx

---

_Assigned to @woodruffw by @woodruffw on 2026-01-13 15:59_

---

_@woodruffw reviewed on 2026-01-13 22:45_

---

_Review comment by @woodruffw on `crates/uv-publish/src/trusted_publishing.rs`:88 on 2026-01-13 22:45_

Flagging: normally I'd consider this an anti-pattern (in terms of untagged enums taking longer to match/producing suboptimal errors on failure), but the context here _might_ make it acceptable:

1. We're in an error path on an already "cold" flow (uploading)
2. The overwhelming majority of users are probably using GitHub publishers, so serde won't need to backtrack for them

OTOH, I could make this explicitly tagged by adding a custom `Deserialize` that peeks the `iss` and dispatches based on that. That wouldn't be hard, AFAIK there's just no way to do it with serde's derive APIs.

---

_@woodruffw reviewed on 2026-01-15 22:38_

---

_Review comment by @woodruffw on `scripts/publish/test_publish.py`:430 on 2026-01-15 22:38_

NOTE: I've removed the previous strategy of polling the index for a "fresh" version to test publishing with, in favor of this "timestamp" strategy where we pick a monotonically increasing (but not contiguous) version.

The main advantage of this is that we don't need to block/retry on the index to get a fresh version, i.e. we spend less time doing something unrelated to the main test functionality itself. The downside is that the versions are a little less nice.



---

_@woodruffw reviewed on 2026-01-15 22:38_

---

_Review comment by @woodruffw on `scripts/publish/test_publish.py`:774 on 2026-01-15 22:38_

The log volume was a bit extreme (and not helpful), so I've dropped it back down to `INFO`.

---
