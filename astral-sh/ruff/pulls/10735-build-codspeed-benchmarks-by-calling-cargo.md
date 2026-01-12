```yaml
number: 10735
title: Build codspeed benchmarks by calling cargo directly
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: manual-codspeed-build
created_at: 2024-04-02T12:19:59Z
updated_at: 2024-04-02T12:59:35Z
url: https://github.com/astral-sh/ruff/pull/10735
synced_at: 2026-01-12T15:55:33Z
```

# Build codspeed benchmarks by calling cargo directly

---

_@MichaReiser_

## Summary

This PR changes the benchmark CI workflow to call `cargo build` directly instead of calling `codspeed build` because
`codspeed` uses a old version of Cargo (1.66) and it resolves feature flags differently, making https://github.com/astral-sh/ruff/pull/10700 not build correctly.

I hope we can migrate back to `codspeed build` once codspeed upgrades its cargo dependency (I've opened a support thread in their discord), but until then lets use this to unblock us

## Test Plan

CI

I set the target of https://github.com/astral-sh/ruff/pull/10700 to this branch and building the benchmarks no longer fails

---

_Label `ci` added by @MichaReiser on 2024-04-02 12:20_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-04-02 12:49_

---

_Review requested from @zanieb by @MichaReiser on 2024-04-02 12:49_

---

_Merged by @MichaReiser on 2024-04-02 12:50_

---

_Closed by @MichaReiser on 2024-04-02 12:50_

---

_Branch deleted on 2024-04-02 12:50_

---

_Comment by @github-actions[bot] on 2024-04-02 12:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
