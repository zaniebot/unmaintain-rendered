```yaml
number: 22508
title: Use rustfmt directly in prek
type: pull_request
state: open
author: charliermarsh
labels:
  - internal
assignees: []
base: main
head: charlie/rustfmt
created_at: 2026-01-11T19:44:36Z
updated_at: 2026-01-12T00:51:45Z
url: https://github.com/astral-sh/ruff/pull/22508
synced_at: 2026-01-12T02:26:21Z
```

# Use rustfmt directly in prek

---

_Pull request opened by @charliermarsh on 2026-01-11 19:44_

## Summary

Apparently this is about ~18x faster (2.25s → 0.12s) for a single-file change, and ~2x faster (2.36s → 1.25s) for `prek run -a`.


---

_Comment by @astral-sh-bot[bot] on 2026-01-11 19:55_


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

_Comment by @AlexWaygood on 2026-01-11 20:11_

You need to change the `SKIP` variable here: https://github.com/astral-sh/ruff/blob/09ff3e705639e4773f185994744390fdeb518e8e/.github/workflows/ci.yaml#L797

---

_Comment by @AlexWaygood on 2026-01-11 20:12_

Though I don't really know why we skip the cargo-fmt pre-commit hook and check rust formatting as a separate CI job 

---

_Marked ready for review by @charliermarsh on 2026-01-12 00:39_

---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-12 00:39_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-12 00:39_

---

_Label `internal` added by @charliermarsh on 2026-01-12 00:39_

---
