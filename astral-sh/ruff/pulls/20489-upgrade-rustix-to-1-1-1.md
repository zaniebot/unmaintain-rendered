```yaml
number: 20489
title: Upgrade rustix to 1.1.1
type: pull_request
state: closed
author: WhyNotHugo
labels: []
assignees: []
base: main
head: rustix-1.1.1
created_at: 2025-09-20T12:57:17Z
updated_at: 2025-09-22T10:53:06Z
url: https://github.com/astral-sh/ruff/pull/20489
synced_at: 2026-01-10T17:40:28Z
```

# Upgrade rustix to 1.1.1

---

_Pull request opened by @WhyNotHugo on 2025-09-20 12:57_

Rustix fails to build on loongarch64 due to ambiguous glob imports for the same symbol. This was fixed in 1.1.1.

Upgrade pinned version to 1.1.1 to fix build failures in transitive dependency.

See: https://github.com/bytecodealliance/rustix/issues/1502
See: https://gitlab.alpinelinux.org/alpine/aports/-/merge_requests/89436



---

_Comment by @github-actions[bot] on 2025-09-20 12:59_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-20 13:09_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-09-20 13:10_

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

_Comment by @MichaReiser on 2025-09-22 10:51_

Thank you. I went ahead and bumped all our transitive dependencies in https://github.com/astral-sh/ruff/pull/20513 This also includes the radix upgrade.

---

_Closed by @MichaReiser on 2025-09-22 10:51_

---

_Comment by @WhyNotHugo on 2025-09-22 10:53_

Thanks!

---
