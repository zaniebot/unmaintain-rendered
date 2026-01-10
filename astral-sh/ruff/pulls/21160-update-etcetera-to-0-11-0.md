```yaml
number: 21160
title: Update etcetera to 0.11.0
type: pull_request
state: merged
author: musicinmybrain
labels:
  - internal
assignees: []
merged: true
base: main
head: etcetera-0.11
created_at: 2025-10-31T06:15:50Z
updated_at: 2025-10-31T12:57:50Z
url: https://github.com/astral-sh/ruff/pull/21160
synced_at: 2026-01-10T16:59:49Z
```

# Update etcetera to 0.11.0

---

_Pull request opened by @musicinmybrain on 2025-10-31 06:15_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This just bumps `etcetera` to the latest version. This has the pleasant effect of removing the `home` crate from the dependency tree. See: https://github.com/lunacookies/etcetera/releases/tag/v0.11.0

## Test Plan

<!-- How was it tested? -->
Tested as a patch for the `ruff` package in Fedora with a proposed `rust-etcetera-0.11.0` update in [COPR](https://copr.fedorainfracloud.org/coprs/music/etcetera/packages/). (Normally I would just compile this locally from a git checkout and run the tests, but my machine with enough memory to compile ruff without workarounds is currently not available.)

---

_Label `internal` added by @MichaReiser on 2025-10-31 12:35_

---

_Comment by @github-actions[bot] on 2025-10-31 12:38_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-31 12:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @MichaReiser on 2025-10-31 12:53_

Thank you

---

_Merged by @MichaReiser on 2025-10-31 12:53_

---

_Closed by @MichaReiser on 2025-10-31 12:53_

---

_Comment by @github-actions[bot] on 2025-10-31 12:57_

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
