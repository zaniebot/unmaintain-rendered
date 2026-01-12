```yaml
number: 21927
title: Prepare 0.14.9 release
type: pull_request
state: merged
author: amyreese
labels:
  - release
assignees: []
merged: true
base: main
head: amy/release-0.14.9
created_at: 2025-12-11T19:39:49Z
updated_at: 2025-12-11T21:17:54Z
url: https://github.com/astral-sh/ruff/pull/21927
synced_at: 2026-01-12T15:57:36Z
```

# Prepare 0.14.9 release

---

_@amyreese_

- **Changelog and docs**
- **metadata**


---

_Review requested from @ntBre by @amyreese on 2025-12-11 19:40_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 19:41_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-11 19:43_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-11 19:59_


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

_@ntBre reviewed on 2025-12-11 20:09_

---

_Review comment by @ntBre on `CHANGELOG.md`:30 on 2025-12-11 20:09_

I think this was missing the `ty` label

---

_@ntBre reviewed on 2025-12-11 20:10_

---

_Review comment by @ntBre on `CHANGELOG.md`:35 on 2025-12-11 20:10_

```suggestion
- [`flake8-bandit`] Fix false positive when using non-standard `CSafeLoader` path (`S506`) ([#21830](https://github.com/astral-sh/ruff/pull/21830))
```

And I'd probably put this in the bug fixes section

---

_@ntBre approved on 2025-12-11 20:10_

---

_Label `release` added by @ntBre on 2025-12-11 20:10_

---

_Merged by @amyreese on 2025-12-11 21:17_

---

_Closed by @amyreese on 2025-12-11 21:17_

---

_Branch deleted on 2025-12-11 21:17_

---
