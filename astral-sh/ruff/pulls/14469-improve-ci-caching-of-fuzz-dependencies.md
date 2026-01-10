```yaml
number: 14469
title: Improve CI caching of fuzz dependencies
type: pull_request
state: closed
author: zanieb
labels:
  - ci
assignees: []
draft: true
base: main
head: zb/fuzz-cache
created_at: 2024-11-20T00:09:10Z
updated_at: 2024-12-30T09:27:45Z
url: https://github.com/astral-sh/ruff/pull/14469
synced_at: 2026-01-10T20:42:26Z
```

# Improve CI caching of fuzz dependencies

---

_Pull request opened by @zanieb on 2024-11-20 00:09_

`cargo fuzz` does not appear to be using cached dependencies, and downloads and compiles everything, e.g., https://github.com/astral-sh/ruff/actions/runs/11923959306/job/33233394417

Presumably this is because the crates are not actually dependencies of this project, which the rust-cache action omits by default.

---

_Label `ci` added by @zanieb on 2024-11-20 00:09_

---

_Comment by @github-actions[bot] on 2024-11-20 00:18_

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

_Comment by @zanieb on 2024-11-20 01:11_

Hm this didn't work, not sure why they're excluded from the cache.

---

_Comment by @MichaReiser on 2024-12-30 09:27_

I'll close this because I assume you aren't working on this at the moment

---

_Closed by @MichaReiser on 2024-12-30 09:27_

---
