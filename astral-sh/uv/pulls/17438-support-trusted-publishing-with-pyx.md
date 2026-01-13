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
updated_at: 2026-01-13T23:04:47Z
url: https://github.com/astral-sh/uv/pull/17438
synced_at: 2026-01-13T23:35:46Z
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

- [ ] Trusted Publishing between GitLab CI/CD <-> PyPI: #17443 
- [ ] Trusted Publishing between GitHub Actions <-> pyx
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
