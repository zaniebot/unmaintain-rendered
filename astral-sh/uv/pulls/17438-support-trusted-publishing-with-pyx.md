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
updated_at: 2026-01-13T22:21:32Z
url: https://github.com/astral-sh/uv/pull/17438
synced_at: 2026-01-13T22:36:21Z
```

# Support Trusted Publishing with pyx

---

_@woodruffw_

## Summary

WIP. This follows #17418 and adds a `PyxPublishingService` that speaks the pyx-specific APIs for Trusted Publishing.

TODOs:

- [ ] Needs a publishing integration test.
- [ ] Needs documentation.
- [ ] Needs changes to `OidcTokenClaims` (these are currently GitHub-specific, they need to become an enum over various supported platforms).

## Test Plan

I'll add new publishing integration tests for the following scenarios:

- [ ] Trusted Publishing between GitLab CI/CD <-> PyPI: #17443 
- [ ] Trusted Publishing between GitHub Actions <-> pyx
- [ ] Trusted Publishing between GitLab CI/CD <-> pyx

---

_Assigned to @woodruffw by @woodruffw on 2026-01-13 15:59_

---
