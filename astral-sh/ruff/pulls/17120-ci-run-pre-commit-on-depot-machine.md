```yaml
number: 17120
title: "CI: Run pre-commit on depot machine"
type: pull_request
state: merged
author: akx
labels:
  - ci
assignees: []
merged: true
base: main
head: ci-chonkier-pre-commit-machines
created_at: 2025-04-01T13:10:23Z
updated_at: 2025-04-01T13:24:20Z
url: https://github.com/astral-sh/ruff/pull/17120
synced_at: 2026-01-10T19:40:37Z
```

# CI: Run pre-commit on depot machine

---

_Pull request opened by @akx on 2025-04-01 13:10_

## Summary

Sibling/alternate of #17108 (see https://github.com/astral-sh/ruff/pull/17108#issuecomment-2769200896).

See if running the pre-commit CI step on Depot machines makes WASM-compiled shellcheck faster.

## Test Plan

> How was it tested?

We're doing it live!

---

_Comment by @akx on 2025-04-01 13:19_

@AlexWaygood The numbers are in (reran this PR and #17108 to warm caches):

* Warm pre-commit run on depot-22.04-16 (this PR): 30 seconds
* Warm pre-commit run on ubuntu-latest w/ native shellcheck (in #17108): 21 seconds 
* Warm pre-commit run on `main` (ubuntu-latest w/ WASI shellcheck): 3m 14 seconds

---

_Marked ready for review by @akx on 2025-04-01 13:19_

---

_Comment by @AlexWaygood on 2025-04-01 13:20_

Woah, nice -- that definitely seems worth it to me!

---

_@AlexWaygood approved on 2025-04-01 13:21_

Thank you! Considering that pre-commit is a required job for automerge to succeed, I'm pretty sure that it's worth it to use the beefier runner here

---

_Label `ci` added by @AlexWaygood on 2025-04-01 13:21_

---

_Merged by @AlexWaygood on 2025-04-01 13:22_

---

_Closed by @AlexWaygood on 2025-04-01 13:22_

---

_Comment by @github-actions[bot] on 2025-04-01 13:22_

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

_Comment by @akx on 2025-04-01 13:22_

I'm also positively surprised by the speedup. I had a hunch that GHA's machines are slow, but not _that_ slow...

The Depot machines are https://depot.dev/, yeah?

---

_Branch deleted on 2025-04-01 13:24_

---

_Comment by @AlexWaygood on 2025-04-01 13:24_

> The Depot machines are [depot.dev](https://depot.dev/), yeah?

not sure, but that looks plausible!

---
