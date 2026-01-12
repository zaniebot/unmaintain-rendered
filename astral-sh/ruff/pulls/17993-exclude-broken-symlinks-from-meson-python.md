```yaml
number: 17993
title: "Exclude broken symlinks from `meson-python` ecosystem check"
type: pull_request
state: merged
author: ntBre
labels:
  - testing
assignees: []
merged: true
base: main
head: brent/remove-meson-python
created_at: 2025-05-09T18:49:22Z
updated_at: 2025-05-09T20:59:02Z
url: https://github.com/astral-sh/ruff/pull/17993
synced_at: 2026-01-12T15:56:09Z
```

# Exclude broken symlinks from `meson-python` ecosystem check

---

_@ntBre_

Summary
--

This should resolve the formatter ecosystem errors we've been seeing lately. https://github.com/mesonbuild/meson-python/pull/728 added the links, which I think are intentionally broken for testing purposes.

Test Plan
--

Ecosystem check on this PR


---

_Label `testing` added by @ntBre on 2025-05-09 18:49_

---

_Comment by @github-actions[bot] on 2025-05-09 18:56_

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

_Comment by @ntBre on 2025-05-09 19:12_

This might be tricky to fix, but I guess you can't actually use `{}` in an `--exclude` glob since the comma causes clap to split the fields.

---

_Review requested from @MichaReiser by @ntBre on 2025-05-09 19:35_

---

_Marked ready for review by @ntBre on 2025-05-09 19:35_

---

_@MichaReiser reviewed on 2025-05-09 20:50_

Thank you

---

_@MichaReiser approved on 2025-05-09 20:50_

---

_Merged by @ntBre on 2025-05-09 20:59_

---

_Closed by @ntBre on 2025-05-09 20:59_

---

_Branch deleted on 2025-05-09 20:59_

---
