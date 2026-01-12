```yaml
number: 9051
title: Fix handling of trailing target comment
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: fix-statement-target-with-trailing-comment
created_at: 2023-12-08T04:34:49Z
updated_at: 2023-12-08T05:00:37Z
url: https://github.com/astral-sh/ruff/pull/9051
synced_at: 2026-01-12T15:55:27Z
```

# Fix handling of trailing target comment

---

_@MichaReiser_

## Summary

This PR fixes an issue where Ruff moved a trailing target comment past the statement end

```python
c = b[dddddd, aaaaaa] =  (
    a[
        aaaaaaa,
        bbbbbbbbbbbbbbbbbbb
    ] 
    # comment 2
) = xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Before

```python
c = b[dddddd, aaaaaa] = a[
    aaaaaaa, bbbbbbbbbbbbbbbbbbb
] = xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
# comment 2
```

Now

```python
c = b[dddddd, aaaaaa] = (
    a[aaaaaaa, bbbbbbbbbbbbbbbbbbb]
    # comment 2
) = xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Which matches black's formatting

## Test Plan

Added test


---

_Comment by @MichaReiser on 2023-12-08 04:35_

Current dependencies on/for this PR:
* main
  * **PR #9051** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9051?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/9051?utm_source=stack-comment).

---

_Label `bug` added by @MichaReiser on 2023-12-08 04:35_

---

_Label `formatter` added by @MichaReiser on 2023-12-08 04:35_

---

_@charliermarsh approved on 2023-12-08 04:45_

---

_Comment by @github-actions[bot] on 2023-12-08 04:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @MichaReiser on 2023-12-08 05:00_

---

_Closed by @MichaReiser on 2023-12-08 05:00_

---

_Branch deleted on 2023-12-08 05:00_

---
