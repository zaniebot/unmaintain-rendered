```yaml
number: 9550
title: "Bump `ring` dependency"
type: pull_request
state: merged
author: WhyNotHugo
labels:
  - internal
assignees: []
merged: true
base: main
head: bump-depends
created_at: 2024-01-16T13:19:55Z
updated_at: 2024-01-16T14:18:09Z
url: https://github.com/astral-sh/ruff/pull/9550
synced_at: 2026-01-12T15:55:29Z
```

# Bump `ring` dependency

---

_@WhyNotHugo_

## Summary

This enables building ruff on [ppc64le](https://en.wikipedia.org/wiki/Ppc64).

See: https://gitlab.alpinelinux.org/alpine/aports/-/issues/15642


## Test Plan

I built ruff on x86_64 and arm64 and ran all tests with it. No failures.

There are no user-facing changes.

---

_Comment by @github-actions[bot] on 2024-01-16 13:39_

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

_Merged by @charliermarsh on 2024-01-16 13:51_

---

_Closed by @charliermarsh on 2024-01-16 13:51_

---

_Comment by @charliermarsh on 2024-01-16 13:51_

Thanks!

---

_Label `internal` added by @charliermarsh on 2024-01-16 13:52_

---

_Branch deleted on 2024-01-16 14:17_

---
