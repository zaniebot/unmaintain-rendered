```yaml
number: 14459
title: Initialize Rust cache before installing binaries and additional components
type: pull_request
state: closed
author: zanieb
labels:
  - ci
assignees: []
draft: true
base: main
head: zb/ci-cache
created_at: 2024-11-19T14:33:27Z
updated_at: 2025-01-22T16:04:13Z
url: https://github.com/astral-sh/ruff/pull/14459
synced_at: 2026-01-10T20:05:43Z
```

# Initialize Rust cache before installing binaries and additional components

---

_Pull request opened by @zanieb on 2024-11-19 14:33_

I was hoping to reduce the time spent on binary installs, but unfortunately additional binaries are always reinstalled https://github.com/taiki-e/install-action/issues/577 — unfortunately that increases the cache size.

However, this does cache setup of the Rust toolchain which may save time. Since most of the Rust toolchain installation is just downloading components, and we still need to download the cache, there might not be much time saved here. The benefit is that the cache is generally closer to the runner on the network. Toolchain installation takes ~10s on Linux and >1m on Windows. It looks like there are some nice savings on Windows but it's less clear on Linux. The pre-commit job is 10s faster, but (perhaps due to the bug above) the test job is 8s slower.

---

_Label `internal` added by @zanieb on 2024-11-19 14:33_

---

_Label `internal` removed by @zanieb on 2024-11-19 14:50_

---

_Label `ci` added by @zanieb on 2024-11-19 14:50_

---

_Comment by @github-actions[bot] on 2024-11-19 14:53_

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

_Closed by @zanieb on 2024-11-19 17:39_

---

_Reopened by @zanieb on 2024-11-19 17:52_

---

_Comment by @MichaReiser on 2024-12-30 09:28_

@zanieb are you planning to work on this? I'd otherwise suggest that we close it or convert it into an issue.

---

_Comment by @zanieb on 2025-01-22 16:01_

Funny, I did this again in #15660 

---

_Closed by @zanieb on 2025-01-22 16:01_

---

_Branch deleted on 2025-01-22 16:04_

---
