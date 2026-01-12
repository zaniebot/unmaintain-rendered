```yaml
number: 8947
title: "Implement the `fix_power_op_line_length` preview style"
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: fix-power-op-line-length
created_at: 2023-12-01T16:23:31Z
updated_at: 2023-12-02T00:35:36Z
url: https://github.com/astral-sh/ruff/pull/8947
synced_at: 2026-01-10T23:40:55Z
```

# Implement the `fix_power_op_line_length` preview style

---

_Pull request opened by @MichaReiser on 2023-12-01 16:23_

## Summary

Implements the `fix_power_op_line_length` preview style. 

The main change is to add a space around the power operator if the binary expression splits over multiple lines:

```python
x = (
    long 
    ** 3
)

# instead of
x = (
    long
    **3
)
```

Closes https://github.com/astral-sh/ruff/issues/8938

## Test Plan

See deleted snapshot file. The similarity index remains unchanged. 



---

_Comment by @MichaReiser on 2023-12-01 16:23_

Current dependencies on/for this PR:
* main
  * **PR #8947** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8947?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8947?utm_source=stack-comment).

---

_Label `formatter` added by @MichaReiser on 2023-12-01 16:25_

---

_Label `preview` added by @MichaReiser on 2023-12-01 16:25_

---

_Marked ready for review by @MichaReiser on 2023-12-01 16:26_

---

_Comment by @github-actions[bot] on 2023-12-01 16:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh approved on 2023-12-01 17:17_

---

_Merged by @MichaReiser on 2023-12-02 00:35_

---

_Closed by @MichaReiser on 2023-12-02 00:35_

---

_Branch deleted on 2023-12-02 00:35_

---
