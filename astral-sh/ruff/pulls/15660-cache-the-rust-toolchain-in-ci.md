```yaml
number: 15660
title: Cache the Rust toolchain in CI
type: pull_request
state: merged
author: zanieb
labels:
  - ci
assignees: []
merged: true
base: main
head: zb/toolchain-cache
created_at: 2025-01-21T23:23:14Z
updated_at: 2025-01-22T07:11:54Z
url: https://github.com/astral-sh/ruff/pull/15660
synced_at: 2026-01-12T15:55:52Z
```

# Cache the Rust toolchain in CI

---

_@zanieb_

We're spending a full 1.5m installing the Rust toolchain on Windows, e.g., https://github.com/astral-sh/ruff/actions/runs/12893749773/job/35950838258

In contrast, in uv this is instant (e.g. https://github.com/astral-sh/uv/actions/runs/12897572530/job/35962989190) because we are caching the toolchain.

This shifts the rust-cache action earlier in all the CI jobs.

---

_Label `internal` added by @zanieb on 2025-01-21 23:23_

---

_Comment by @github-actions[bot] on 2025-01-21 23:31_

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

_Comment by @zanieb on 2025-01-21 23:43_

Looks to have shaved a bit of time off the Windows job https://github.com/astral-sh/ruff/actions/runs/12897912866/job/35963963939?pr=15660

Incredible that running `rustup show` takes 18s still — just to report the installed toolchain?

---

_Comment by @zanieb on 2025-01-21 23:47_

Appears to come out about the same on Linux jobs

---

_@dhruvmanila approved on 2025-01-22 04:17_

---

_Comment by @zanieb on 2025-01-22 06:26_

It's down to the expected 0 seconds on Windows with #15664, I think.

---

_Merged by @zanieb on 2025-01-22 07:10_

---

_Closed by @zanieb on 2025-01-22 07:10_

---

_Branch deleted on 2025-01-22 07:10_

---

_Label `internal` removed by @MichaReiser on 2025-01-22 07:11_

---

_Label `ci` added by @MichaReiser on 2025-01-22 07:11_

---
