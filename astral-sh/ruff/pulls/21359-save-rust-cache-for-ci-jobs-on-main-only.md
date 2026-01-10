```yaml
number: 21359
title: Save rust cache for CI jobs on main only
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: micha/rust-cache-main-only
created_at: 2025-11-10T08:45:25Z
updated_at: 2025-11-10T09:04:46Z
url: https://github.com/astral-sh/ruff/pull/21359
synced_at: 2026-01-10T16:53:55Z
```

# Save rust cache for CI jobs on main only

---

_Pull request opened by @MichaReiser on 2025-11-10 08:45_

GitHub action imposes a cache limit per repository of 10GB. We consistently exceed this limit by a lot (currently ±80GB). GitHub starts deleting caches when the limit is exceeded. Because of that, it's important that we're deliberate about which artifacts we cache. 

This PR disables caching Rust builds from PRs. Instead, we rely on main to publish warm caches that are then used by all PRs. This should reduce our cache pressure and can also help speeding up the CI jobs because writing the caches often takes multiple seconds (~10-30s but sometimes more).

---

_Label `ci` added by @MichaReiser on 2025-11-10 08:45_

---

_Comment by @astral-sh-bot[bot] on 2025-11-10 08:56_


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

_Marked ready for review by @MichaReiser on 2025-11-10 08:57_

---

_Renamed from "Save rust cache for CI jobs on main onlyu" to "Save rust cache for CI jobs on main only" by @AlexWaygood on 2025-11-10 08:59_

---

_@sharkdp approved on 2025-11-10 09:02_

Thank you.

---

_Merged by @MichaReiser on 2025-11-10 09:04_

---

_Closed by @MichaReiser on 2025-11-10 09:04_

---

_Branch deleted on 2025-11-10 09:04_

---
